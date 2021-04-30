# js_quoted_pritable_vcf
Javascript quict quoted printable implementation (i.e. for vcf)


```js
function quoted_printable_encode(str) {
  return unescape(encodeURIComponent(str)).split("").map((x,i)=>"="+(x.charCodeAt(0).toString(16).toUpperCase()+(i%23==22?"\r\n":""))).join("");
};

function quoted_printable_decode(str) {
  return decodeURI(escape(b.replace(/\s/g,"").split("=").splice(1).map(x=>String.fromCharCode(parseInt(x,16))).join("")))
};
```
