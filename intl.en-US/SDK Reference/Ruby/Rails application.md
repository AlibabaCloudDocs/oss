# Rails application

This topic describes how to use OSS SDK for Ruby in Rails applications to list buckets, upload objects, and download objects.

## Prerequisites

To use OSS SDK for Ruby in Rails applications, you must add the following dependency to `Gemfile`.

```
gem 'aliyun-sdk', '~> 0.3.0
			
```

Add the following dependency when you use OSS SDK for Ruby.

```
require 'aliyun/oss'
			
```

## Integration with Rails

You can perform the following steps to integrate OSS SDK for Ruby with Rails.

1.  Create a project.
    1.  Install Rails and create a Rails application named oss-manager.

        ```
        gem install rails
        rails new oss-manager
        							
        ```

        **Note:** In this example, the oss-manager application is a management tool for OSS objects. It can list all buckets, list objects in a bucket by folders, upload objects, and download objects.

    2.  Use git to manage the project code.

        ```
        cd oss-manager
        git init
        git add .
        git commit -m "init project"
        							
        ```

2.  Add the dependency on OSS SDK for Ruby.
    1.  Edit the `oss-manager/Gemfile` file to add the dependency on OSS SDK for Ruby.

        ```
        gem 'aliyun-sdk', '~> 0.3.0'
        							
        ```

    2.  Run the `bundle install` command under `oss-manager/`.
    3.  Save the changes.

        ```
        git add .
        git commit -m "add aliyun-sdk dependency"
        							
        ```


## Initialize the OSS client

To avoid initialization each time when you use the OSS client in the project, we recommend that you create an initialization file in the project.

```language-ruby
# oss-manager/config/initializers/aliyun_oss_init.rb
require 'aliyun/oss'

module OSS
  def self.client
    unless @client
      Aliyun::Common::Logging.set_log_file('./log/oss_sdk.log')

      @client = Aliyun::OSS::Client.new(
        endpoint:
          Rails.application.secrets.aliyun_oss['endpoint'],
        access_key_id:
          Rails.application.secrets.aliyun_oss['access_key_id'],
        access_key_secret:
          Rails.application.secrets.aliyun_oss['access_key_secret']
      )
    end

    @client
  end
end
			
```

The preceding code can be found in the rails/ directory of the installation path of OSS SDK for Ruby. You can use the OSS client in the project after initialization.

```
buckets = OSS.client.list_buckets
			
```

The endpoint, AccessKey ID, and AccessKey secret are saved in the `oss-manager/conf/secrets.yml` file.

```language-yaml
development:
  secret_key_base: xxxx
  aliyun_oss:
    endpoint: xxxx
    access_key_id: aaaa
    access_key_secret: bbbb
			
```

Save the changes.

```
git add .
git commit -m "add aliyun-sdk initializer"
			
```

## List all buckets

You can perform the following steps to list all buckets.

1.  Use Rails to generate the controller to manage buckets.

    ```
    rails g controller buckets index
    					
    ```

2.  Generate the following files in the `oss-manager` directory.
    -   app/controller/buckets\_controller.rb: contains the logical code related to buckets.
    -   app/views/buckets/index.html.erb: contains the code used to display the content related to buckets.
    -   app/helpers/buckets\_helper.rb: defines the auxiliary functions.
3.  Edit the buckets\_controller.rb file and call the OSS client to save the result of `list_buckets` to the `@buckets` variable.

    ```language-ruby
    class BucketsController < ApplicationController
      def index
        @buckets = OSS.client.list_buckets
      end
    end
    					
    ```

4.  Edit the views/Buckets/index.html.erb file to list the bucket list.

    ```language-html
    <h1>Buckets</h1>
    <table class="table table-striped">
      <tr>
        <th>Name</th>
        <th>Location</th>
        <th>CreationTime</th>
      </tr>
    
      <% @buckets.each do |bucket| %>
      <tr>
        <td><%= link_to bucket.name, bucket_objects_path(bucket.name) %></td>
        <td><%= bucket.location %></td>
        <td><%= bucket.creation_time.localtime.to_s %></td>
      </tr>
      <% end %>
    </table>
    					
    ```

5.  Define the `bucket_objects_path` auxiliary function in the app/helpers/buckets\_helper.rb file.

    ```language-ruby
    module BucketsHelper
      def bucket_objects_path(bucket_name)
        "/buckets/#{bucket_name}/objects"
      end
    end
    					
    ```

    After the preceding steps are complete, you can list all buckets.

    In addition, you must configure the routing of Rails to make sure that the address you enter in the browser can use the correct logic. Edit the `config/routes.rb` file and add the following content:

    ```
    resources :buckets do
      resources :objects
    end
    					
    ```

    Enter `rail s` under `oss-manager/` to start the Rails server, and then enter `http://localhost:3000/buckets/` in the browser to view the bucket list.

6.  Save the changes.

    ```
    git add .
    git commit -m "add list buckets feature"
    					
    ```


## List all objects in a bucket by folders

You can perform the following steps to list all objects in a bucket by folders:

1.  Generate a controller to manage objects.

    ```
    rails g controller objects index
    					
    ```

2.  Edit the app/controllers/objects\_controller.rb file.

    ```language-ruby
    class ObjectsController < ApplicationController
      def index
        @bucket_name = params[:bucket_id]
        @prefix = params[:prefix]
        @bucket = OSS.client.get_bucket(@bucket_name)
        @objects = @bucket.list_objects(:prefix => @prefix, :delimiter => '/')
      end
    end
    					
    ```

    You can obtain the bucket name from the parameters in the URL. Add a prefix to list objects by folders and call the list\_object operation of the OSS client to obtain the object list.

    **Note:** In this example, objects whose names contain the specified prefix and a forward slash \(/\) as a delimiter are obtained. For more information, see [Manage objects](/intl.en-US/SDK Reference/Ruby/Manage objects.md).

3.  Edit the app/views/objects/index.html.erb file.

    ```language-html
    <h1>Objects in <%= @bucket_name %></h1>
    <p> <%= link_to 'Upload file', new_object_path(@bucket_name, @prefix) %></p>
    <table class="table table-striped">
      <tr>
        <th>Key</th>
        <th>Type</th>
        <th>Size</th>
        <th>LastModified</th>
      </tr>
    
      <tr>
        <td><%= link_to '../', with_prefix(upper_dir(@prefix)) %></td>
        <td>Directory</td>
        <td>N/A</td>
        <td>N/A</td>
      </tr>
    
      <% @objects.each do |object| %>
      <tr>
        <% if object.is_a?( Aliyun::OSS::Object) %>
        <td><%= link_to remove_prefix(object.key, @prefix),
                @bucket.object_url(object.key) %></td>
        <td><%= object.type %></td>
        <td><%= number_to_human_size(object.size) %></td>
        <td><%= object.last_modified.localtime.to_s %></td>
        <% else  %>
        <td><%= link_to remove_prefix(object, @prefix), with_prefix(object) %></td>
        <td>Directory</td>
        <td>N/A</td>
        <td>N/A</td>
        <% end  %>
      </tr>
      <% end %>
    </table>
    					
    ```

    Objects are listed by folder based on the following logic:

    1.  The content before the first forward slash \(/\) in the object name is considered as the upper-level folder.
    2.  Common prefixes are listed as folders.
    3.  Objects are displayed as files.
    The auxiliary functions used in the preceding code, such as `with_prefix` and `remove_prefix` are defined in the app/helpers/objects\_helper.rb file.

    ```language-ruby
    module ObjectsHelper
      def with_prefix(prefix)
        "? prefix=#{prefix}"
      end
    
      def remove_prefix(key, prefix)
        key.sub(/^#{prefix}/, '')
      end
    
      def upper_dir(dir)
        dir.sub(/[^\/]+\/$/, '') if dir
      end
    
      def new_object_path(bucket_name, prefix = nil)
        "/buckets/#{bucket_name}/objects/new/#{with_prefix(prefix)}"
      end
    
      def objects_path(bucket_name, prefix = nil)
        "/buckets/#{bucket_name}/objects/#{with_prefix(prefix)}"
      end
    end
    					
    ```

4.  Run `rails s` and enter `http://localhost:3000/buckets/my-bucket/objects/` in the browser to view the object list.
5.  Save the changes.

    ```
    git add .
    git commit -m "add list objects feature"
    					
    ```


## Download objects

In the steps in the preceding section, URLs are added to the objects. You can use the URL of an object to download the object.

```language-html
<td><%= link_to remove_prefix(object.key, @prefix),
        @bucket.object_url(object.key) %></td>
				
```

You can also use `bucket.object_url` to generate a temporary URL for the object you want to download. For more information, see [Download objects](/intl.en-US/SDK Reference/Ruby/Download objects.md).

## Upload objects

In Rails applications on the server, you can use the following methods to upload objects:

-   Upload objects in a way similar to normal upload. You must first upload an object to the Rails server. Then, the Rails server uploads the object to OSS.
-   Upload objects by using the forms and temporary credentials generated by the server.
    1.  Add the `#new` method to the app/controllers/objects\_controller.rb file to generate upload forms.

        ```language-ruby
        def new
          @bucket_name = params[:bucket_id]
          @prefix = params[:prefix]
          @bucket = OSS.client.get_bucket(@bucket_name)
          @options = {
            :prefix => @prefix,
            :redirect => 'http://localhost:3000/buckets/'
          }
        end
        								
        ```

    2.  Edit the app/views/objects/new.html.erb file.

        ```language-html
        <h2>Upload object</h2>
        
        <%= upload_form(@bucket, @options) do %>
          <table class="table table-striped">
            <tr>
              <td><label>Bucket:</label></td>
              <td><%= @bucket.name  %></td>
            </tr>
            <tr>
              <td><label>Prefix:</label></td>
              <td><%= @prefix  %></td>
            </tr>
        
            <tr>
              <td><label>Select file:</label></td>
              <td><input type="file" name="file" style="display:inline" /></td>
            </tr>
        
            <tr>
              <td colspan="2">
                <input type="submit" class="btn btn-default" value="Upload" />
                <span>&nbsp;&nbsp</span>
                <%= link_to 'Back', objects_path(@bucket_name, @prefix) %>
              </td>
            </tr>
          </table>
        <% end %>
        								
        ```

        **Note:** `upload_form` is an auxiliary function provided by OSS SDK for Ruby. It is used to generate upload forms and is defined in the rails/aliyun\_oss\_helper.rb file. You must copy the function to the app/helpers/ directory. Then, you can run `rails s` and access `http://localhost:3000/buckets/my-bucket/objects/new` to upload objects to OSS.

    3.  Save the changes.

        ```
        git add .
        git commit -m "add upload object feature"
        								
        ```


## Add styles

You can perform the following steps to add styles to the interface of the Rails application.

1.  Download [bootstrap](http://getbootstrap.com/). Decompress the package and then copy the `bootstrap.min.css` file to the `app/assets/stylesheets/` directory.
2.  Modify the `app/views/layouts/application.html.erb` file and change the `yield` line to the following content.

    ```language-html
      <div id="main">
        <%= yield %>
      </div>
    						
    ```

3.  Add a `<div>` field whose ID is main for each page. Modify the `app/assets/stylesheets/application.css` file and add the following contents.

    ```language-css
    body {
        text-align: center;
    }
    
    div#main {
        text-align: left;
        width: 1024px;
        margin: 0 auto;
    }
    						
    ```


For the complete demo code, visit [OSS Rails Demo](https://github.com/aliyun/alicloud-oss-rails-demo).

