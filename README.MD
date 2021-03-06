**Api Consumer** by [<span style="color: red; font-size: 18pt; font-family: consolas">*Armane*</span>](http://armane.studio "Armane")
 
**Using API Consumer with Java, Android & Kotlin**

#  Implimentation 

The API Consumer is a tool that can be used perfectly to make http request in Java and kotlin applications.
To use the API Consumer visit  and follow the instructions.
 
------------
 ##******With Gradle******
 <br/>
 you can use this API with Gradle (**Android** Or **Desktop** or **Web**).
 
To get a Git project into your build:

 
+ Step 1. Add the JitPack repository to your **_build.gradle_** at the end of repositories:

```groovy 
	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
 ```

+ Step 2. Add the dependency

```groovy
	dependencies {
	        implementation 'com.github.Abderrahmanearache:ApiConsumer:1.1'
	} 
```

	
<br/> 

------------
##******With Maven****** 
<br/>
you can use this API also with Maven Repository 
 
To get a Git project into your Maven Project:

 
  + Step 1.
  Add the JitPack repository to your **_pom.xml_** repositories. 
  
```xml
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
 ```
 + Step 2. Add the dependency
 
 
```xml
	<dependency>
	    <groupId>com.github.Abderrahmanearache</groupId>
	    <artifactId>ApiConsumer</artifactId>
	    <version>1.1</version>
	</dependency>
```
 
 
# How it works 


The API contains many features that help you get data from your APIs or external APIs.

So far you can get your data as a single string or json, so ApiConsumer provides 2 methods :
- A `StringRequest` that you can use to get simple text,
- A `JsonRequest` that allows you to get your data as a `JSONObject` to parse at the end.

 ### >> Getting a simple text 

to get a simple String data from an API, using a StringRequest :
 

```java
 
        StringRequest sRequest = new StringRequest("http://armane.studio/apiconsumer/params")
                .onFinish(response -> System.out.println(response))
                .onError(System.err::println);
        sRequest.execute();

```

at the end, you get a `StringResponse` which contains a` getResponse` function returns a string value as below

  ```java 
         StringRequest sRequest = new StringRequest("http://armane.studio/apiconsumer/params")
                .onFinish(response -> {
                    System.out.println(response.getResponse());
                });
  ```
------------


### Methods
 

generally, the method is GET by default, unless you define the method in the parameters.

```java
	StringRequest sRequest = new StringRequest("http://armane.studio/apiconsumer/params",Method.POST);
```
or by executing the request by `doGet()`,`doPost() `or` doRequest()` instead of `execute() `


------------
### Binding params 


to send parameters to your API, using GET or POST:
- use `addParams` with a String matrix :

```java
        StringRequest sRequest = new StringRequest("http://armane.studio/apiconsumer/params", Method.POST);
        sRequest.addParams(new String[][]{
                {"name1", "value1"},
                {"name2", "value2"},
				...
		});
		
```

- or use  `addParams` with a `HashMap<String,String>` :

```java
			
        HashMap<String, String> params = new HashMap<>();
        params.put("name1", "value1");
        params.put("name2", "value2");

        StringRequest sRequest = new StringRequest("http://armane.studio/apiconsumer/params", Method.POST);
        sRequest.addParams(params);
```

- or using `addJsonParam` with  an new or existing object directly
 ```java
        StringRequest sRequest = new StringRequest("http://armane.studio/apiconsumer/params", Method.POST);
        sRequest.addJsonParam(new Object() {
            String name1 = "value1";
            String name2 = "value2";
            Object object = new Object() {
                        String name = "name";
                        String job = "job";   
               }; 
        });
        sRequest.doRequest();        
```

this method parse the object and stringify it as bellow :
```json
{
   "name1":"value1",
   "name2":"value2",
   "object":{
      "name":"name",
      "job":"job"
   }
}
```
if you want use `addJsonParam`, make sure  you use the doRequest method, the json data goes at input to your api,
***to read it in your servier side :*** 
 <br/>
+- **java servlet**  >> use` request.getReader()` <br/>
+- **PHP** >>use `file_get_contents('php://input')` 

------------


### Result

The `onFinish` function is a callback function that can be executed when the request ends perfectly and returns a result, so we have to define what we needs to do at the end.

The `onError` function is a callback function that can be executed when an error has occurred somewhere during execution.

```java
      StringRequest sRequest = new StringRequest("http://armane.studio/apiconsumer/params")
                .onFinish(response -> {
                    doWhatYouWantWith(response.getResponse());
                })
                .onFinish(error -> {
                    showError(error.gerMessage());
                });
```

 ### >> Getting a Json Object 

if you know that your API will provide you with a json format, it is recommended to use JsonRequest instead.

the JsonRequest has the **same** functions as StringRequest, the only diferent is that you make sure that you get a **jsonObject** at the end.


Exemple : 
```java

        HashMap<String, String> hashParam = new HashMap<>();
        hashParam.put("name1", "value1");
        hashParam.put("name2", "value2");

        JsonRequest jsonRequest = new JsonRequest("http://armane.studio/apiconsumer/params")
        jsonRequest.addParams(hashParam)
                //or
                .addParams(new String[][]{
                        {"name1", "value1"},
                        {"name2", "value2"},
                })
		//or
                .addJsonParam(new Object() {
                    String param1 = "value1";
                    String name2 = "value2";
                    Object object = new Object() {
                    		String name = "name";
                    		String job = "job";   
                   }; 
                }) 
                .onFinish(jsonResponse -> {
                    if (jsonResponse.isJson())
                        System.out.println(jsonResponse.getJsonObject().get("keyFromYourJsonResult"));
                })
                .onError(System.err::println);
        //to execute 
        jsonRequest.execute(); //or
        jsonRequest.doPost(); //or
        jsonRequest.doGet(); //or
        jsonRequest.doRequest(); //or 
```

you can use it with `Kotlin` language with Android 
Example :

```kotlin
        var req = JsonRequest("http://armane.studio/apiconsumer/params")
                .onFinish {
            		jsonResponse -> Log.d("json", jsonResponse.jsonObject.toString())
        		}
                .onError {
                    exception -> exception.printStackTrace()
                }
                .addParams(arrayOf(
                        arrayOf("key", "val"),
                        arrayOf("key2", "val2")
                ))
        req.doGet()

 ```
 
