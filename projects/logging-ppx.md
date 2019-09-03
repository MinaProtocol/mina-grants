# Logging PPX

**Project Category:** OCaml

**Level:** Intermediate

**Skills Required:** OCaml, knowledge of [PPX's](https://ocamlverse.github.io/content/ppx.html)

**Problem:**

Calls to the logger abstraction are verbose and contain boilerplate in between each logging call. Specifically, the same "magic bindings" (e.g. `__MODULE__`) are passed in to each logging call. This boilerplate can be removed by implementing a PPX for logging. Using a PPX can also enable making logging calls even more concise by representing metadata inclusion as interpolation.

**Proposed Solution:**

Below is an example of what logging looks like in the code right now, as well as what it could look like with a PPX that removes "magic binding" boilerplate, followed by a PPX that also supports metadata interpolation syntax.

```ocaml
(* this is what it looks like now *)
Logger.info logger ~module_:__MODULE ~location:__LOC__
  ~metadata:[("state_hash", State_hash.to_yojson state_hash)]
  "Received state hash $state_hash"

(* this is what it would look like with the magic values stripped *)
[%log_info logger
  ~metadata:[("state_hash", State_hash.to_yojson state_hash)]
  "Received state hash $state_hash"]

(* this is what it would look like with magic bindings stripped + metadata interpolation *)
[%log_info logger "Received state hash ${State_hash.t: state_hash}"]

(* an alternative syntax which may be easier to implement *)
[%log_info "Received state hash ${State_hash.t}"] logger state_hash
```

**Milestones:**

Part 1:
* Strip magic bindings and replace with PPX
* Grant available: $200 

Part 2:
* Support metadata interpolation - eg. don’t have to explicitly define metadata values, and implicitly converts to yojson
* Grant available: $200
