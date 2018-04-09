#How a cross-site scripting attack works (JAVA)
---
In a cross-site scripting (XSS) attack, the attacker injects malicious code into a legitimate web page that then runs a client-side script. When a user visits the infected web page, the script is downloaded to the user's browser.

Suppose the attacker injects this HTML string into a web page:

`<script>alert("You've been attacked!")</script>`

When the browser loads the web page, it runs the script as part of rendering the page. In this case, the script runs, and the user sees an alert pop up that says "You've been attacked!

---
##Your defense: Encoding HTML in variables in a server-side Java™ application

To ensure that malicious scripting code is not injected into your page, your best line of defense is to encode all variable strings before they're displayed on the page. Encoding merely means converting every potentially dangerous character to an HTML entity.

The HTML string above will look like this when escaped: Listing 1. Escaped HTML

`&lt;script&gt;alert(&quot;You&apos;ve been attacked!&quot;)&lt;/script&gt;`

##Your turn! Try changing the code in Listing 1

```
import java.lang.StringBuilder;
import java.util.HashMap;

public class EscapeUtils {

  public static void main(String[] args) {
    String unescapedText =
      "<script>alert(\"You've been attacked!\")</script>";
    System.out.println(escapeHtml(unescapedText));
  }

  public static String escapeHtml(String inputString) {
    StringBuilder builder = new StringBuilder();
    char[] charArray = inputString.toCharArray();
    for (char nextChar: charArray) {
      String entityName = charMap.get((int) nextChar);
      if (entityName == null) {
        if (nextChar > 0x7F)
          builder.append("&#")
            .append(Integer.toString(nextChar, 10))
            .append(";");
        else
          builder.append(nextChar);
      }
      else
        builder.append(entityName);
    }
    return builder.toString();
  }

  public static final HashMap<Integer, String> charMap =
    new HashMap<Integer, String>();

  static {
    charMap.put(34, "&quot;");    // double quote
    charMap.put(35, "&#35;");     // hash mark (no HTML named entity)
    charMap.put(38, "&amp;");     // ampersand
    charMap.put(39, "&apos;");    // apostrophe, aka single quote
    charMap.put(60, "&lt;");      // less than
    charMap.put(62, "&gt;");      // greater than
  }
}                    
```

#HttpUtility.HtmlDecode Method (String) (C#)
```
using System;
using System.Web;
using System.IO;

   class MyNewClass
   {
      public static void Main()
      {
         String myString;
         Console.WriteLine("Enter a string having '&' or '\"'  in it: ");
         myString=Console.ReadLine();
         String myEncodedString;
         // Encode the string.
         myEncodedString = HttpUtility.HtmlEncode(myString);
         Console.WriteLine("HTML Encoded string is "+myEncodedString);
         StringWriter myWriter = new StringWriter();
         // Decode the encoded string.
         HttpUtility.HtmlDecode(myEncodedString, myWriter);
         Console.Write("Decoded string of the above encoded string is "+
                        myWriter.ToString());
      }
   }
```

  