Disclaimer: This application is on the list of apps that I expect people to tell me not to build or stop building. 
<br/>Not conventional to the javascript way. 
<br/>Take this app with a grain of salt. 
<br/>It was an experiment and a bunch of fun 😉

## Start with Lessons Learned:
I ended up using this application to map the project out but did not actually have the project refer to its own node_modules folder. Instead the scripts section of the package.json in every application I was going to use had `cd -P .;` prepended to every script. This small command says ... if im in a simlink go to the original folder. Which would arrive at the node_modules folder and look to its neigbor node_modules instead of its children node_modules.

Warning: take care if you link your node_modules localy pre publishing them as this script should not be implemented under that circumstance. Therefore you should probably store these as production only scripts in the package.json.

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

## Usage in your project:
Everything actually runs in the node_modules folder; as long as `cd -P .;` is prepended to your package.json scripts.

## Analysis
"node_modules_mapped" contains symlinks to your "node_modules" projects, making the program very lightweight.

The size of the "node_modules_mapped" folder for "aerial_desktop" implementation was 83 bytes.
<br/>The overhead for "aerial_desktop" implementation is around 1 second.


### 1 second is pretty slow! Symlinking is not the culprit.
Symlink Node Module Mapper has a command:
```bash
str=$(ls | grep -v -E '(^aerial_desktop$|^dyna-dynamic-walpaper$)' | xargs -I {} echo "$pathy/node_modules/{}");
```
1) grep ignores using regular expressions.
2) string concats the path to the result of the grep command (expensive). 

Additionally, Aerial Desktop uses electron so it has a large number of packages that this command must address.

But if we were copying those files it would take 30 seconds, sooo ... 😉
<br/>There are some additions that will optimize this slowness that are on the imaginary roadmap.
