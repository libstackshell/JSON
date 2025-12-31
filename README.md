# JSON
json functions

```JSON
{
  "company": "spacelift",
  "domain": ["devops", "devsecops"],
  "tutorial": [
    { "yaml": { "name": "YAML Ain't Markup Language", "type": "awesome", "born": 2001 } },
    { "json": { "name": "JavaScript Object Notation", "type": "great", "born": 2001 } }
  ],
  "author": "omkarbirade",
  "published": true
}
```

```Javascript
function main() {
	
	var f = std.fopen("test.json", "r");
	var data = std.fread(f);
	std.fclose(f);
	
	print("== RAW JSON ==\n" + data + "\n");

    // -------- parse --------
    var doc = json.parse(data);

    // -------- get / has --------
    print("== json.get / json.has ==");
    print("company exists? " + json.has(doc, "company"));
    print("company = " + json.get(doc, "company"));

    print("domain[0] exists? " + json.has(doc, "domain[0]"));
    print("domain[0] = " + json.get(doc, "domain[0]"));

    print("tutorial[0].yaml.name = " + json.get(doc, "tutorial[0].yaml.name"));
    print("");

    // -------- count --------
    print("== json.count ==");
    print("root count (object keys) = " + json.count(doc));
    print("domain count (array) = " + json.count(json.get(doc, "domain")));
    print("");

    // -------- keys --------
    print("== json.keys (root object) ==");
    var k = json.keys(doc);
    print(k);   // ArrayList Darstellung deiner Runtime
    print("");

    // -------- type checks --------
    print("== json.is_obj / json.is_arr ==");
    print("doc is obj? " + json.is_obj(doc));
    print("doc is arr? " + json.is_arr(doc));
    print("domain is arr? " + json.is_arr(json.get(doc, "domain")));
    print("");

    // -------- iter root object --------
    print("== json.iter over ROOT (Object) ==");
    var it = json.iter(doc);
    while (json.next(it)) {
        // object: index = -1, key != null
        print("key=" + json.key(it) + " idx=" + json.index(it) + " val=" + json.value(it));
    }
    print("");

    // -------- iter sequence (domain) --------
    print("== json.iter over domain (Array) ==");
    it = json.iter(json.get(doc, "domain"));
    while (json.next(it)) {
        // array: key = null, index >= 0
        print("idx=" + json.index(it) + " val=" + json.value(it));
    }
    print("");

    // -------- set: einfache Werte --------
    print("== json.set ==");
    json.set(doc, "company", "NEWCO");
    json.set(doc, "published", false);
    json.set(doc, "domain[1]", "security");

    print("company now = " + json.get(doc, "company"));
    print("published now = " + json.get(doc, "published"));
    print("domain[1] now = " + json.get(doc, "domain[1]"));
    print("");

    // -------- set: ArrayList -> JsonArray --------
    json.set(doc, "numbers", [1, 2, 3]);
    print("numbers count = " + json.count(json.get(doc, "numbers")));

    it = json.iter(json.get(doc, "numbers"));
    while (json.next(it)) {
        print("numbers[" + json.index(it) + "]=" + json.value(it));
    }
    print("");

    // -------- json.node / json.string --------
    print("== json.node / json.string ==");
    var node = json.node([ "a", "b", "c" ]);   // JsonArray
    print(json.string(node));
    print("");

    it = json.iter(node);
    while (json.next(it)) {
        var idx = json.index(it);
        if (idx >= 0)
            print("[" + idx + "] => " + json.value(it));
        else
            print(json.key(it) + " => " + json.value(it));
    }

    // -------- final dump --------
    print("== FINAL JSON ==");
    print(json.string(doc));
	
}
```
