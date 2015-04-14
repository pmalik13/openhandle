[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle C Code Examples ##

Here's a simple C program (contained in file "[openhandle.c](http://nurture.nature.com/tony/openhandle/code/c/openhandle.c)") which can be used to grab a handle data record (uses the [JSON-C](http://oss.metaparadigm.com/json-c/) library):
```
#include <stdlib.h>
#include <string.h>
#include <curl/curl.h>
#include <json/json.h>

// Callback funtion - handles data returned
size_t write_json(void *ptr, size_t size, size_t nmemb, void *stream)
{
    size_t totalSize = nmemb * size;
    char *json = malloc(totalSize + 1);
    struct json_object *new_obj;

    strcpy(json, (char *) ptr);

    printf("json = \n%s\n", json);

    new_obj = json_tokener_parse(json);
    printf("new_obj.to_string() = %s\n", json_object_to_json_string(new_obj));
    json_object_put(new_obj);

    return 0;
}

// Entry point - reads command line args and despatches
int main(int argc, char *argv[])
{
    const char *OPENHANDLE = "http://nascent.nature.com/openhandle/handle?&format=json&id=";
    char uri[255];

    CURL *curl;
    CURLcode res;
 
    if (argc != 2) {
        printf("Usage: %s <handle>\n", argv[0]);
    }
    else {
        strcpy(uri, OPENHANDLE);
        strcat(uri, argv[1]);

        curl = curl_easy_init();
        if (curl) {
            curl_easy_setopt(curl, CURLOPT_URL, uri);
            curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, write_json);
            res = curl_easy_perform(curl);
            curl_easy_cleanup(curl);
        }
        return 0;
    }
}
```

Compiling this
```
% gcc -l curl -l json openhandle.c -o openhandle
```
and running gives the following (note that this is still a work in progress and so doesn't produce the standard [Hello World](OpenHandleHelloWorld.md) document):
```
% ./openhandle 10100/nature
json = 
{
    "comment" : "OpenHandle (JSON) - see http://openhandle.googlecode.com/" ,
    "handle" : "hdl:10100/nature" ,
    "handleValues" : [
        {
            "index" : "100" ,
            "type" : "HS_ADMIN" ,
        "data" : {
                "adminRef" : "hdl:10100/nature?index=100" ,
                "adminPermission" : "111111111111"
            } ,
            "permission" : "1110" ,
            "ttl" : "+86400" ,
            "timestamp" : "Wed Feb 28 15:37:06 GMT 2007" ,
            "reference"  : []
        } ,
        {
            "index" : "1" ,
            "type" : "URL" ,
            "data" : "http://www.nature.com/" ,
            "permission" : "1110" ,
            "ttl" : "+86400" ,
            "timestamp" : "Wed Feb 28 15:37:06 GMT 2007" ,
            "reference"  : []
        }
    ]
}

new_obj.to_string() = { "comment": "OpenHandle (JSON) - see http:\/\/openhandle.googlecode.com\/", "handle": "hdl:10100\/nature", "handleValues": [ { "index": "100", "type": "HS_ADMIN", "data": { "adminRef": "hdl:10100\/nature?index=100", "adminPermission": "111111111111" }, "permission": "1110", "ttl": "+86400", "timestamp": "Wed Feb 28 15:37:06 GMT 2007", "reference": [ ] }, { "index": "1", "type": "URL", "data": "http:\/\/www.nature.com\/", "permission": "1110", "ttl": "+86400", "timestamp": "Wed Feb 28 15:37:06 GMT 2007", "reference": [ ] } ] }
```