!SLIDE title
# A JSON API<br/>to bind them all #
<div class="title_desc">
  <ul>
    <li><a href="http://twitter.com/ebastien">Emmanuel Bastien</a></li>
    <li><a href="http://rivierarb.fr">Riviera.rb</a></li>
    <li>June 13, 2013</li>
  </ul>
</div>

!SLIDE
## Common web frameworks ##
* Good at simple CRUD interfaces
* Pretty bad at supporting RESTful APIs

!SLIDE
## REST... Hypermedia ##
* HTML, anchor tags!
* XML, link tags with rel and href attributes?
* JSON ?!

!SLIDE
## JSON API ##
* A community-driven standard for RESTful JSON APIs
* ID-based, easier to start with
* URL-based, more resilient to changes
* Early development stage but very active

!SLIDE
## (Partial) Implementations ##
* [Ember Data](https://github.com/emberjs/data)
* [Active Model Serializers](https://github.com/rails-api/active_model_serializers)

!SLIDE
## JSON with reserved keys ##
    meta, links, href, type, id

!SLIDE
## A media type (IANA registration pending) ##
    application/vnd.api+json

!SLIDE
## Tailored with profile link header: [rfc6906](http://tools.ietf.org/html/rfc6906) ##
<pre>
HTTP/1.1 200 OK

Content-Type: application/vnd.api+json

Link: &lt;http://example.com/profile>;
      rel="profile"
</pre>
* Similar to HTML microformats
* Avoid proliferation of media types
* Allow self-descriptive messages

!SLIDE
## An API for Riviera.rb ##
<pre>
GET http://rivierarb.fr/api

Accept: application/vnd.api+json
</pre>

!SLIDE
<pre>
HTTP/1.1 200 OK

Content-Type: application/vnd.api+json

Link: &lt;http://rivierarb.fr/api/profile>;
      rel="profile"
</pre>

    @@@javascript
    {
      "api": [{
        "links": {
          "posts": "http://rivierarb.fr/api/posts",
          "people": "http://rivierarb.fr/api/people"
        }
      }]
    }

!SLIDE
<pre>GET http://rivierarb.fr/api/posts</pre>
    @@@javascript
    {
      "posts": [{
        "id": "1",
        "title": "A JSON API to bind them all",
        "href": "http://rivierarb.fr/api/posts/1"
      }]
    }

!SLIDE
## Relationships ##
    @@@javascript
    {
      "posts": [{
        "id": "1",
        "title": "A JSON API to bind them all",
        "links": {
          "comments":
            "http://rivierarb.fr/api/posts/1/comments"
        }
      }]
    }

!SLIDE
## URI Templates: [rfc6570](http://tools.ietf.org/html/rfc6570) ##
    @@@javascript
    {
      "links": {
        "posts.author":
          "http://rivierarb.fr/api/people/{posts.author}"
      },
      "posts": [{
        "id": "1",
        "title": "A JSON API to bind them all",
        "links": { "author": "6" }
      }, {
        "id": "2",
        "title": "RuLu 2013, June 20-21, Lyon",
        "links": { "author": "9" }
      }]
    }

!SLIDE
## Compounds ##
    @@@javascript
    {
      "links": {
        "posts.author": {
          "href": "http://.../people/{posts.author}",
          "type": "people"
        }
      },
      "posts": [{
        "id": "1",
        "title": "A JSON API to bind them all",
        "links": { "author": "6" }
      }],
      "people": [{"id": "6", "name": "rivierarb"}]
    }

!SLIDE
## References ##
* [jsonapi.org](http://jsonapi.org/)
* [The 'profile' Link Relation and You](http://www.designinghypermediaapis.com/blog/the-profile-link-relation-and-you.html)
* [rfc6906: The 'profile' Link Relation Type](http://tools.ietf.org/html/rfc6906)
* [rfc6570: URI Template](http://tools.ietf.org/html/rfc6570)
* [Ember Data](https://github.com/emberjs/data)
* [Active Model Serializers](https://github.com/rails-api/active_model_serializers)
