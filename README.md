# js_quoted_pritable_vcf
Javascript quick quoted printable implementation (i.e. for vcf)


```js
  // use wrap="=\r\n " for vcf format and Linear White SPace LWSP
  
function quoted_full_printable_encode(str,wrap=false,max=76) {
  // https://pl.wikipedia.org/wiki/Quoted-Printable
  // https://tools.ietf.org/html/rfc2045#section-6.7
  max=~~(max / 3); wrap=wrap===false?"=\r\n":wrap;
  return unescape(encodeURIComponent(str)).split("").map((x,i)=>"="+(x.charCodeAt(0).toString(16).toUpperCase()+(i%max==(max-1)?wrap:""))).join("");
};

function quoted_printable_encode(str,wrap=false,max=76) {
  // https://pl.wikipedia.org/wiki/Quoted-Printable
  // https://tools.ietf.org/html/rfc2045#section-6.7
  // 33 through 60 inclusive, and 62 through 126> 9=/t 32_space
  let len=0; wrap=wrap===false?"=\r\n":wrap;
  const quote=(c)=>"="+c.toString(16).toUpperCase();

  return unescape(encodeURIComponent(str)).split("").map((x,i)=>{
    let c=x.charCodeAt(0);
    if ((c>=33 && c<=60) || (c>=62 && c<=126) || ((c==32 || c==9) && len && len<max-1)) {
      len++
      if (len>=max) {len=0;return wrap+x};
      return x;
    }
    else {
      len+=3;
      if (len>=max) {len=0;return wrap+quote(c)};
      return quote(c)
    }
  }).join("");
};

function quoted_printable_decode(str) {
  return  decodeURI(escape(str.replaceAll(/(=\r\n\s*)/g,"").replaceAll(/(=(..))/g,(m,r1,r2)=>String.fromCharCode(parseInt(r2,16)))));
};


```
