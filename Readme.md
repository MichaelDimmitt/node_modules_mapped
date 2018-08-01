Disclaimer: This application is on the list of apps that I expect people to tell me not to build or stop building. 
<br/>Not conventional to the javascript way. 
<br/>Take this app with a grain of salt. 
<br/>It was an experiment and a bunch of fun 😉

## The Simlink Node Module Mapper
low/no overhead, symlink node_modules. Tell the package dependency path.

for when projects depend on packages that depend on packages that depend on packages ...

<pre>
package v1
  dependent package of v1 named v2
    dependent package of v2 named v3
    dependent package of v2 named v3
package x1
  dependent package of x1 namded x2
</pre>

This is totally possible I am implementing it in npm-Aerial-Desktop
