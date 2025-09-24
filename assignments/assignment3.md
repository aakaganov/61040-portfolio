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
1) 
<b>concept</b> Analytics [ShortUrl]<br>
  <b>purpose</b> track number of times each short URL is accessed<br>
  <b>principle</b> every lookup of a short URL increases its count<br>
  <b>state</b><br>
    a set of Records with<br>
    <ul>
      shortUrl String<br>
      count Number<br>
   </ul>
  <b>actions</b><br>
    increment (shortUrl: String)<br>
      <ul><b>effect</b> increases the count for shortUrl by 1<br></ul>
    getCount (shortUrl: String): (count: Number)<br>
      <ul><b>equires</b> a record exists for shortUrl<br>
      <b>effect</b> returns the count for shortUrl<br></ul>

<b>​​concept</b> Ownership [ShortUrl]</b><br>
  <b>purpose</b> restrict analytics visibility to the user who created the short URL</b><br>
  <b>principle</b> only the owner of a short URL can view its analytics</b><br>
  <b>state</b><br>
    a set of Owners with<br>
    <ul>
      userId String<br>
      shortUrl String<br></ul>
  <b>actions</b><br>
    assign (userId: String, shortUrl: String)<br>
      <ul><b>effect</b> records that userId is the owner of shortUrl<br></ul>
    checkOwner (userId: String, shortUrl: String): (ok: Boolean)<br>
      <ul><b>effect</b> returns true if userId owns shortUrl, else false<br></ul>
2) 
**Shortening created**:<br>
<b>sync</b> initAnalyticsAndOwnership<br>
<b>when</b><br>
  <ul>Request.shortenUrl (targetUrl, shortUrlBase, userId)<br>
  UrlShortening.register (): (shortUrl)<br></ul>
<b>then</b><br>
  <ul>Analytics.increment (shortUrl)<br>              
  Ownership.assign (userId, shortUrl)<br></ul>

**Shortenings translated to targets**: <br>
<b>sync</b> trackLookup<br>
<b>when</b> 
    <ul>UrlShortening.lookup (shortUrl)<br></ul>
<b>then</b> 
    <ul>Analytics.increment (shortUrl)<br></ul>

**Requests analytics**: <br>
<b>sync</b> viewAnalytics<br>
<b>when</b><br>
  <ul>Request.getAnalytics (shortUrl, userId)<br>
  Ownership.checkOwner (userId, shortUrl): (ok)<br></ul>
<b>then</b><br>
  <ul>if ok then Analytics.getCount (shortUrl)<br></ul>

3) 
<b>Allowing users to choose their own short URLs</b> : <br>
<b>Using the “word as nonce” strategy to generate more memorable short URLs</b> : <br>
<b>Including the target URL in analytics, so that lookups of different short URLs can be grouped together when they refer to the same target URL </b> : <br>
<b>Generate short URLs that are not easily guessed </b>: <br>
<b>Supporting reporting of analytics to creators of short URLs who have not registered as user </b>: <br>
