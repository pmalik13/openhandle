[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Java Code Examples ##

Here's how a native Java program (contained in file "[OpenHandle.java](http://nurture.nature.com/tony/openhandle/code/java/OpenHandle.java)") can grab a handle data record (using the [JSON Reader](http://www.stringtree.org/stringtree-json.html) from stringtree.org):
```
% cat OpenHandle.java
import java.net.*;
import java.io.*;
import java.util.*;
import org.stringtree.json.JSONReader;

public class OpenHandle {

  private static final String OPENHANDLE = "http://nascent.nature.com/openhandle/handle";

  public static void main(String args[]) {

    URL url;
	String line;
    StringBuffer buffer = new StringBuffer();
	String json = new String();

    try {
	  url = new URL(OPENHANDLE + "?format=json&id=" + args[0]);
      try {
	    BufferedReader in = new BufferedReader(
				              new InputStreamReader(
				                url.openStream()));

	    while ((line = in.readLine()) != null)
          buffer.append(line + "\n");
	   
	    json = buffer.toString();
	    in.close();
      }
      catch (Exception e) {
        System.err.println(e);
      }
    }
    catch (Exception e) {
      System.err.println(e);
    }

    JSONReader reader = new JSONReader();

    System.out.println(reader.read(json));
  }
}
```

Compiling this
```
% javac OpenHandle.java
```
and running gives the following:
```
% java OpenHandle 10100/nature
{handle=hdl:10100/nature, handleValues=[{index=100, ttl=+86400, type=HS_ADMIN, data={adminRef=hdl:10100/nature?index=100, adminPermission=111111111111},
permission=1110, timestamp=Wed Feb 28 15:37:06 GMT 2007, reference=[]}, {index=1, ttl=+86400, type=URL, data=http://www.nature.com/, permission=1110,
timestamp=Wed Feb 28 15:37:06 GMT 2007, reference=[]}], comment=OpenHandle (JSON) - see http://openhandle.googlecode.com/}
```