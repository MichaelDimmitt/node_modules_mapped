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

## Preliminary Thoughts:
Should you do this ... probably not. But it was fun to build.
<br/>I think it removes complexity as it gives you a path to understand how everything is connected.

Allows you to better understand how your node_modules folder got large quick and keeps it simple in node_modules_mapped folder.
<br/>Currently does not scale and does require a few steps from developers.
<br/>Next step would be to scale it from reading the dependencies in package.json simular to how npm reads them.
<br/>Maybe I can get npm to tell me the dependency structure and build it from that.

Use of `cd -P .;` prepended to all scripts in package.json ensures the app we are running is not a symlink but the folder in the node_modules.
<br/>It should not negatively impact the projecy and can be incorporated into any project.

## Preliminary script that worked for aerial.
```bash
## paste into terminal
rm -rf *; npm install aerial_desktop; pathy=~/i_am_death_thedestroyer_of_worlds; cd node_modules/; mkdir aerial_desktop/node_modules; mkdir dyna-dynamic-walpaper/node_modules; mkdir electron/node_modules; cd ..; mkdir node_modules_mapped && ln -s $pathy/node_modules/aerial_desktop node_modules_mapped/; ln -s $pathy/node_modules/dyna-dynamic-walpaper node_modules/aerial_desktop/node_modules;  cd node_modules/; str=$(ls | grep -v -E '(^aerial_desktop$|^dyna-dynamic-walpaper$)' | xargs -I {} echo "$pathy/node_modules/{}"); ln -s $str dyna-dynamic-walpaper/node_modules/; cd ..;

## this script was added to package.json, link coming soon.
 "dyna": "cd -P .; cd ../dyna-dynamic-walpaper; electron-packager . --overwrite; open dyna-dynamic-walpaper-darwin-x64/dyna-dynamic-walpaper.app"
```

### final product:
After npm install has occoured, due to a self executing bin script.

Your application will contain, 
<pre>
node_modules
node_modules_mapped
</pre>

![what have i done 😱](https://user-images.githubusercontent.com/11463275/43528334-b5f1dafa-9576-11e8-83dc-1f4e1d7e960a.png)
