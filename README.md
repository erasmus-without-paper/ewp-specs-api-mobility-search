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

Parameters MUST be provided in the regular `application/x-www-form-urlencoded`
format.


### `sending_hei_id` (required)

An identifier of the institution which is the master of the Outgoing Mobility
objects we want to search through. This parameter MUST be required by the
server even if it covers only a single institution. It is *not* repeatable.


### `limit` (optional)

Positive integer of `none`. Default is **20**.

If `none` is given, the server is REQUIRED to return **all** Mobility objects
(match the criteria imposed by other parameters, if any were given). This list
can be huge, but it will allow the clients to fetch the complete list of
Mobility identifiers (to facilitate full replication).


### `iia_id` (repeatable, optional)

A list of Interinstitutional Agreement identifiers. If given, then the results
returned will be filtered and they MUST contain only such Outgoing Mobility
objects whose IIA matches **any** of the `iia_id` identifiers.

This parameter is *repeatable*, so the request MAY contain multiple occurrences
of it. The server is REQUIRED to process all of them.


### `receiving_hei_id` (repeatable, optional)

A list of institution identifiers. If given, then the results returned MUST
contain only such Outgoing Mobility objects whose receiving institution matches
**any** of the `receiving_hei_id` identifiers.

This parameter is *repeatable*, so the request MAY contain multiple occurrences
of it. The server is REQUIRED to process all of them.


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
   accessible (to the caller) via its Outgoing Mobilities API.

 * Server MUST NOT publish IDs of objects which are NOT accessible via its
   Outgoing Mobilities API. Clients which receive IDs from this API should be
   able to fetch all objects referenced by those IDs.


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply.

 * Note, that parameters are "AND-ed" together, but their values are "OR-ed".
   This means that invalid or unknown values provided in `iia_id`,
   `sending_hei_id` and `receiving_hei_id` parameters MUST be **allowed** (but
   **not ignored**) by the server.
   
   The difference between "being allowed" and "being ignored" becomes clear
   once you try to analyze the scenario when one of these lists contains
   **only** such IDs which are unknown to the server. The server MUST respond
   with an empty `<response>` element in this case, regardless of all the other
   parameters. For example:
   
   * `?sending_hei_id=known&iia_id=known&iia_id=UNKNOWN` will return some
     results,
   * `?sending_hei_id=known&iia_id=known` will return exactly the same results,
   * `?sending_hei_id=known&iia_id=UNKNOWN` will **NEVER** return any results,
   * `?sending_hei_id=known` will return results again (though not the same).


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
