# ueditor-blazor

A wysiwyg rich text Web editor based on UEditor and Blazor.

# Usage

1. Install the package
    ```bash
      $ dotnet add package UEditor
    ```

2. Import js resources
    ```html
        <script src="_content/UEditor/neditor.config.js"></script>
        <script src="_content/UEditor/neditor.all.min.js" defer></script>
        <script src="_content/UEditor/ueditor-blazor.js"></script>
    ```

3. That's all! Then you can use the `UEditor.Editor` component.
    ```razor
    <UEditor.Editor @ref="editor" @bind-Value="value" @bind-Html="html" Height="500" MinHeight="500" />
    
    @code {
        string value = "Hello Blazor!";
        string html;
    
        Editor editor;
    }
    ```

# Image Loading

If you want to implement custom image loading, follow the instructions listed below:

1. Make sure there are some configuration in config file: **neditor.config.js**, **neditor.service.js**.

    ```js
        serverUrl: "$YOUR_POST_URL", //About Line 31
    ```

    ```js
        UE.Editor.prototype.getActionUrl = function(action) {
            if (action == "uploadimange") {
                return '$YOUR_POST_URL'; //About Line 10-11
            }
            ... //rest of codes
        }
    ```

2. According to

    [ueditor]: http://fex.baidu.com/ueditor/#dev-request_specification	"UEditor Documentation # 后端请求规范"
    
    , json in uploadimage action response will be this:
    
    ```json
        {
        	"state": "SUCCESS",
        	"url": "upload/demo.jpg",
    	"title": "demo.jpg",
        	"original": "demo.jpg"
	}
    ```
    
    After implementation like above json format response, there will be no images inserted into your editor.
    
    **neditor.server.js** line 60 in *getResponseSuccess* method requires there will be a **code** field in json:
    
    ```json
        {
            "state": "SUCCESS",
            "url": "upload/demo.jpg",
            "title": "demo.jpg",
            "original": "demo.jpg",
            "code": 200
        }
    ```

3. Enjoy.