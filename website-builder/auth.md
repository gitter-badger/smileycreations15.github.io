<h2>Authorize</h2>
<p id="auth-status">Authorizing...</p>
<script>
function getQueryVariable(variable){
   var query = window.location.search.substring(1);
   var vars = query.split("&");
   for (var i=0;i<vars.length;i++) {
           var pair = vars[i].split("=");
           if(pair[0] == variable){return decodeURIComponent(pair[1]);}
   }
   return(null);
}
                                 function escapeHtml(unsafe) {
    return unsafe
         .replace(/&/g, "&amp;")
         .replace(/</g, "&lt;")
         .replace(/>/g, "&gt;")
         .replace(/"/g, "&quot;")
         .replace(/'/g, "&#039;");
 }
   if (sessionStorage.getItem("state-github-basic-auth") === null){
  document.getElementById("auth-status").innerHTML = "Authorization failed.<br>Local <code>state</code> not set.<br>File a <a href='https://github.com/smileycreations15/smileycreations15.github.io/issues/new'>issue</a> with the error message for more info."
                                   history.replaceState({},"Authorization failure",window.location.pathname)
}
if (sessionStorage.getItem("state-github-basic-auth") !== getQueryVariable("state")){
  document.getElementById("auth-status").innerHTML = "Authorization failed.<br><code>state</code> parameter does not match.<br>File a <a href='https://github.com/smileycreations15/smileycreations15.github.io/issues/new'>issue</a> with the error message for more info."
                                   history.replaceState({},"Authorization failure",window.location.pathname)
} else {
  document.getElementById("auth-status").innerHTML = "Processing token..."
  fetch("https://smileycreations15.wixsite.com/backend/_functions/api_key_github?api-key=" + encodeURIComponent(getQueryVariable("code")),{"method":"POST"})
  .then(a=>{return a.json()})
  .then(data=>{
  if (data.error === "unknown"){
    document.getElementById("auth-status").innerHTML = "A error occured while creating a API key.<br>File a <a href='https://github.com/smileycreations15/smileycreations15.github.io/issues/new'>issue</a> with the error message for more info."
   history.replaceState({},"Authorization failure",window.location.pathname)
  } else {
   localStorage.setItem("cookie-github",data.cookie)
   localStorage.setItem("github-username",data.login)
   
   localStorage.setItem("github-scope",data.scope)
   window.location.href = "https://smileycreations15.com/website-builder/1"
  }
  }).catch(e=>{
  document.getElementById("auth-status").innerHTML = "A error occured. <br>" + e.toString() + "<br>File a <a href='https://github.com/smileycreations15/smileycreations15.github.io/issues/new'>issue</a> with the error message for more info."
   history.replaceState({},"Authorization failure",window.location.pathname)
  })
   history.replaceState({},"Authorizing...",window.location.pathname)
}
</script>
