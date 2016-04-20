Outgoing Mobility Search API
============================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This document describes the **Outgoing Mobility Search API**. This API is
implemented by the sending institution. It allows the receiving institution to
access a list of all Outgoing Mobility objects associated with it (with some
basic filtering options).


Request method
--------------

 * Requests MUST be made with either HTTP GET or HTTP POST method. Servers MUST
   support both these methods. Servers SHOULD reject all other request methods.

 * Clients are advised to use POST when passing large number of parameters
   (servers MAY set a limit on their GET query string length).


Request parameters
------------------

Parameters MUST be provided either in a query string (for GET requests), or in
the `application/x-www-form-urlencoded` format (for POST requests).


### `sending_hei_id` (repeatable, optional)

A list of institution identifiers. If given, then the results return MUST
contain only these Outgoing Mobility objects the sending institution of which
matches with **any** of these identifiers.

This parameter is *repeatable*, so the request MAY contain multiple occurrences
of it. The server is REQUIRED to process all of them.

It the parameter is omitted, then the server MUST NOT filter the response based
on the sending institution. (It has exactly the same meaning as passing all HEI
IDs associated with this EWP Host.)


### `receiving_hei_id` (repeatable, optional)

This parameter has exactly the same meaning as the `sending_hei_id` parameter,
the only difference being that it is the *receiving* institutions which are
filtered by it (not the sending ones).


### (more parameters to come)

More parameters will be added here after the model of the Outgoing Mobility
is specified.


Permissions
-----------

All requests from the EWP Network MUST be allowed access to this API. Consult
the [Echo API][echo] specs for details on handling unprivileged requests.

However, only selected Outgoing Mobility objects should be accessible to the
caller. Visibility of objects SHOULD be kept in sync with the visibility
described in the Outgoing Mobilities API. That is:

 * Servers MUST publish IDs of every Outgoing Mobility object which is
   accessible via its Outgoing Mobilities API.

 * Server MUST NOT publish IDs of objects which are NOT accessible via its
   Outgoing Mobilities API. Clients which receive IDs from this API should
   always be able to fetch objects referenced by these IDs via the Outgoing
   Mobilities API.


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply.

 * Invalid (or unknown) `sending_hei_id` and `receiving_hei_id` values MUST be
   ignored by the server, i.e. servers MUST still respond with a a valid HTTP
   200 XML response. If *none* of the values on at least one of these lists is
   known, then servers MUST respond with an empty envelope.


Response
--------

Servers MUST respond with a valid XML document described by the [response.xsd]
(response.xsd) schema. See the schema annotations for further information.


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[echo]: https://github.com/erasmus-without-paper/ewp-specs-api-echo
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
[institutions-api]: https://github.com/erasmus-without-paper/ewp-specs-api-institutions
