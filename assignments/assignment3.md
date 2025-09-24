# Assignment 3
## Concepts for URL Shortening
1) Contexts are a way to allow for separate sections of uniqueness. From a global perspective, not every string needs to be unique, just unique within their own context. The contexts are used in the URL shortening app in order to only require uniqueness for different short URL bases. For example the string home would be valid for both tinyurl.com/abc and myshort.ly/abc

2) NonceGeneration stores a set of strings in order to ensure uniqueness by being able to keep track of what strings have already been used. The set of used strings is related to the counter because the set of strings corresponds to integers from 0 to n - 1, the counter represents the same information as the set of strings but in a more compact way.
3) One advantage of using a word from the dictionary for nonce generation is that it is easier for the user to remember and type out. One disadvantage of this scheme is that the space for unique strings is smaller, meaning collisions happen sooner. <br>

<b>concept</b> WordNonceGeneration \[Context\]<br>
<b>purpose</b> generate unique strings within a context with each string being a common dictionary word<br>
<b>principle</b> each generate returns a string not returned before for that context, and that string is a word<br>
<b>state</b><br>
a set of Contexts with<br>
    <ul>
      a used set of Words<br>
      a dictionary set of Words<br>
    </ul>
<b>actions</b><br>
generate (context: Context): (nonce: Word)<br>
<ul>
    <b>requires</b> there to exist words in the dictionary for this context that have not already been used<br>
    <b>effect</b> chooses a word from dictionary of this context not already used, returns it, and adds it to the used set<br>
</ul>  

## Synchronizations for URL Shortening
1) Generate only needs to generate a nonce that is not already used in that context, this means the targetUrl argument is not needed because it does not link the nonce and the targetUrl. With that being said, it does need the shortUrlBase for the context. Register must have both targetUrl and ShortUrlBase because UrlShortening.register must know where to attach the nonce(ShortUrlBase) and where to map the new url(targetUrl)
2) This convention is not used in every case because there are cases when names are different, possibility of collisions, and there is a need for explicitness. When the names are different they must be specified because they need to be known. If multiple actions produce the same variable name, the naming convention prevents ambiguity. Sometimes names are written in order to be as explicit as possible. 
3) Request.shortenUrl is included in the first two syncs because those syncs are triggered from incoming requests for a new short URL. The third sync, setExpiry, is triggered by a shortening being registered. This means it only listens for UrlShortening.register. 
4) Once the need to support multiple domain bases is gone, the need for shortUrlBase is gone because shortUrlBase is constant. Instead, in this case, ‘bit.ly’ can be used instead. 
5) sync handleExpiry
when ExpiringResource.expireResource (): (resource)
then UrlShortening.delete (shortUrl: resource)

## Extending the design
1) f
2) f
3) f
