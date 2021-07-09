# Rails应用

本文主要介绍如何在Rails应用中使用OSS Ruby SDK来列举存储空间（Bucket）、上传文件（Object）、下载文件等。

## 准备工作

在Rails应用中使用OSS Ruby SDK，需要在`Gemfile`中添加以下依赖。

```
gem 'aliyun-sdk', '~> 0.3.0
            
```

然后在使用OSS时引入以下依赖。

```
require 'aliyun/oss'
            
```

## 与Rails集成

OSS Ruby SDK与Rails集成的流程如下。

1.  创建项目
    1.  安装Rails，然后创建一个oss-manager的Rails应用。

        ```
        gem install rails
        rails new oss-manager
                                    
        ```

        **说明：** 本示例中的oss-manager是OSS的文件管理器，包含列举所有Bucket、按目录层级列举Bucket下所有文件、上传文件和下载文件等功能。

    2.  使用Git管理项目代码。

        ```
        cd oss-manager
        git init
        git add .
        git commit -m "init project"
                                    
        ```

2.  添加SDK依赖
    1.  编辑`oss-manager/Gemfile`，并加入SDK的依赖。

        ```
        gem 'aliyun-sdk', '~> 0.3.0'
                                    
        ```

    2.  在`oss-manager/`中执行`bundle install`。
    3.  保存更改。

        ```
        git add .
        git commit -m "add aliyun-sdk dependency"
                                    
        ```


## 初始化OSS Client

为了避免在项目中用到OSS Client时都需要初始化，建议在项目中添加一个初始化文件。

```
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

以上代码可以在SDK的rails/目录下找到。初始化后，可以在项目中方便地使用OSS Client。

```
buckets = OSS.client.list_buckets            
```

其中endpoint、access\_key\_id和access\_key\_secret都保存在`oss-manager/conf/secrets.yml`中。

```
development:
  secret_key_base: xxxx
  aliyun_oss:
    endpoint: xxxx
    access_key_id: aaaa
    access_key_secret: bbbb           
```

保存更改。

```
git add .
git commit -m "add aliyun-sdk initializer"            
```

## 列举所有的Bucket

您可以按照如下步骤列举所有的Bucket。

1.  用Rails生成管理Bucket的controller。

    ```
    rails g controller buckets index
                        
    ```

2.  在`oss-manager`中生成以下文件。
    -   app/controller/buckets\_controller.rb Buckets相关的逻辑代码
    -   app/views/buckets/index.html.erb Buckets相关的展示代码
    -   app/helpers/buckets\_helper.rb辅助函数
3.  编辑buckets\_controller.rb，调用OSS Client将`list_buckets`的结果存放在`@buckets`变量中。

    ```
    class BucketsController < ApplicationController
      def index
        @buckets = OSS.client.list_buckets
      end
    end
    ```

4.  编辑views/buckets/index.html.erb，列举Bucket列表。

    ```
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

5.  在app/helpers/buckets\_helper.rb中定义辅助函数`bucket_objects_path`。

    ```
    module BucketsHelper
      def bucket_objects_path(bucket_name)
        "/buckets/#{bucket_name}/objects"
      end
    end
                        
    ```

    以上步骤完成后即可列出所有的Bucket。

    此外，您需要配置Rails的路由，使得在浏览器中输入的地址能够调用正确的逻辑。编辑`config/routes.rb`时需增加如下内容。

    ```
    resources :buckets do
      resources :objects
    end
                        
    ```

    在`oss-manager/`下输入`rails s`以启动Rails服务器，然后在浏览器中输入`http://localhost:3000/buckets/`即可查看Bucket列表。

6.  保存更改。

    ```
    git add .
    git commit -m "add list buckets feature"
                        
    ```


## 按目录层级列举Bucket下的所有文件

按目录层级列举Bucket下的所有文件的步骤如下。

1.  生成一个管理Object的controller。

    ```
    rails g controller objects index
                        
    ```

2.  编辑app/controllers/objects\_controller.rb。

    ```
    class ObjectsController < ApplicationController
      def index
        @bucket_name = params[:bucket_id]
        @prefix = params[:prefix]
        @bucket = OSS.client.get_bucket(@bucket_name)
        @objects = @bucket.list_objects(:prefix => @prefix, :delimiter => '/')
      end
    end
                        
    ```

    您可以从URL的参数中获取Bucket名字。您需要添加一个前缀来实现只按目录层级列出Object，然后调用OSS Client的list\_objects接口获取文件列表。

    **说明：** 这里获取的是指定前缀下并且以正斜线（/）为分界的文件。具体操作，请参见[管理文件](/intl.zh-CN/SDK 示例/Ruby/管理文件.md)。

3.  编辑app/views/objects/index.html.erb。

    ```
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
        <% if object.is_a?(Aliyun::OSS::Object) %>
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

    将文件按目录结构显示的实现逻辑是：

    1.  总是在第一个出现'../'时，指向上级目录。
    2.  对于Common prefix，显示为目录。
    3.  对于Object，显示为文件。
    上面的代码中使用了`with_prefix`、`remove_prefix`等辅助函数，这些辅助函数定义在app/helpers/objects\_helper.rb中。

    ```
    module ObjectsHelper
      def with_prefix(prefix)
        "?prefix=#{prefix}"
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

4.  完成之后运行`rails s`，然后在浏览器中输入地址`http://localhost:3000/buckets/my-bucket/objects/`即可查看文件列表。
5.  保存更改。

    ```
    git add .
    git commit -m "add list objects feature"                    
    ```


## 下载文件

在上述显示文件列表步骤中已为每个文件添加了URL，您可以直接使用该文件URL来下载文件。

```
<td><%= link_to remove_prefix(object.key, @prefix),
        @bucket.object_url(object.key) %></td>               
```

您还可以使用`bucket.object_url`为文件生成临时的URL。具体操作，请参见[下载文件](/intl.zh-CN/SDK 示例/Ruby/下载文件.md)。

## 上传文件

在Rails服务端应用中，您可以通过以下两种方式上传文件。

-   第一种上传方式与普通上传文件类似。您需要先将文件上传到Rails服务器，服务器再将文件上传到OSS。
-   第二种上传方式是使用服务器生成的表单和临时凭证将文件直接上传到OSS。
    1.  在app/controllers/objects\_controller.rb中增加`#new`方法，用于生成上传表单。

        ```
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

    2.  编辑app/views/objects/new.html.erb。

        ```
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

        **说明：** `upload_form`是SDK提供的一个便于生成上传表单的辅助函数，该辅助函数存放在rails/aliyun\_oss\_helper.rb中。您需要将其拷贝到app/helpers/目录下，完成后运行`rails s`，然后通过访问`http://localhost:3000/buckets/my-bucket/objects/new`即可将文件上传到OSS。

    3.  保存更改。

        ```
        git add .
        git commit -m "add upload object feature"                               
        ```


## 添加样式

您可以按照如下步骤为界面添加样式。

1.  下载[bootstrap](http://getbootstrap.com/)，解压后将 `bootstrap.min.css`拷贝到`app/assets/stylesheets/`下。
2.  修改`app/views/layouts/application.html.erb`，并将`yield`这一行改成。

    ```
      <div id="main">
        <%= yield %>
      </div>
                            
    ```

3.  为每个页面添加一个ID为main的`<div>`，然后修改 `app/assets/stylesheets/application.css`，并添加以下内容。

    ```
    body {
        text-align: center;
    }
    
    div#main {
        text-align: left;
        width: 1024px;
        margin: 0 auto;
    }
                            
    ```


完整的demo代码请参见[OSS Rails Demo](https://github.com/aliyun/alicloud-oss-rails-demo)。

