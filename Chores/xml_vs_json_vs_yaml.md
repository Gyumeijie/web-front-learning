# XML vs JSON vs YAML
Here are some considerations you might use to guide your decision:

## XML
- You have to represent mixed content (tags mixed within text). This would appear to be a major concern in your case. You might even 
consider HTML for this reason.
- You need to transform the data to another XML/HTML format. (XSLT is great for transformations.)

## JSON
- You have to represent data records, and a closer fit to JavaScript is valuable to your team or your community.
> The lack of comments in JSON is, in practice, a real pain.

## YAML (Technically YAML is a superset of JSON)
- You have to represent data records, and you value some additional features missing from JSON: comments, strings without quotes,
order-preserving maps, and extensible data types.

> 
> Additionally, if you have to represent data records, and you value ease of import/export with databases and spreadsheets, then **CSV**
may be a better choice.
