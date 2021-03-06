<!--Topic description-->
<description>Learn about the template controller and its lifecycle events</description>

## Rendering data in the template area
Let's say we need to render the user's first and last names on the template area.<br>
Here is how we do it:

### index.html
The following template markup does not include the required scripts (jquery and hi-es5). It serves only as a demonstration:
````html    
    <html>
        <head>
           <!-- There is stuff missing in here -->      
        </head>
        <body>             
            <a href="#">
                 <!--Here we render the user's full name-->
                 {{user.firstName}} {{user.lastName}}
            </a>        
            <div id="view_content">{{view_content}}</div>                
        </body>
    </html>  
````


### index.js
The template controller declaration:

````js

    Hi.template({
    
        user:{
           firstName:"John",
           lastName:"Doe"
        }
    
    });
    
````

What we have done so far works perfectly, the only caveat is that we want the user data to be coming from the server-side,
we don't want to hardcode it because it's nonsense. That's exactly what we are going to do next.


## From the server-side straight to the template controller

The only way to pass data from the server-side straight to the template controller is listening to a CDI event as follows:

```java
     
    @ApplicationScoped
    public class Whatever {
    
    
         @Inject
         private FrontEnd frontEnd;          
               
         //Will be fired every time the template is loaded
         public void doMagic(@Observes TemplateLoadEvent event){
            
               UserInfo info = //fetch user data
               Map data = new HashMap();
               data.put("user",info);        
               frontEnd.setTemplateData(data);           
         
         }    
    
    }

```

The __TemplateLoadEvent__ is fired every time the page is loaded, which means, the user data will always be there.



## Accessing the current view from the template

If for some reason you need to access the $scope of the current view from the template controller, here is how to proceed:

```js
    
    Hi.template({
    
        yourFunction : function(){
            
            var currentView =  this.$activeView;
            //do whatever you want here
        
        }
    
    }


```


    
## Reaching the template controller from a view controller

If for example you need to access the user details from a view, you could just get them from the template controller as follows:

```java

    var user = Hi.$template.user;
    //do whatever you want here


```

> **NOTICE**<br> You can access the __template controller__ from any script using the approach presented above, not just __from views__.

