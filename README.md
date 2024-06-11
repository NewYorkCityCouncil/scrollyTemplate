# scrollyTemplate

Template borrowed from https://github.com/onsvisual/svelte-scrolly, look at documentation and demo there too for help (most code is inherited from that repository).
NOTE: included /node_modules/ in repository because I customized some files in @onsvisual/svelte-charts/src/charts/

Other helpful links:
- https://svelte.dev/
- https://layercake.graphics/
- https://github.com/sveltejs/svelte-scroller

Directions:
- Recommend using Visual Studio Code (VS Code)
- Clone this repository to VS Code
- Open terminal window in VS Code
- Type "npm run dev" in terminal (may need to download "npm")---spits out a local link that shows the webpage as you work on it (save your work and it will update the webpage)
- Type "npm run build" in terminal when finished with webpage, this will compile it (compiled code in /public folder)
- Al can then make a council webpage with the compiled html code 

File explanations:
- /src/App.svelte: This is the main code file. Some example code is currently in the file
- 
- /src/layout/Scroller.svelte: Call when making a Scrolly section, see examples in App.svelte. Probably don't need to change code in this file, see https://github.com/sveltejs/svelte-scroller for details
- /src/layout/Table.svelte: Call when making a table. See example in App.svelte
- /node_modules/@onsvisual/svelte-charts/src/charts/BarChart.svelte: Call when making a bar chart, see examples in App.svelte. ONS made this file and files for other chart types (see /node_modules/@onsvisual/svelte-charts/src/charts/ folder). I tweaked this file (and builder files it calls) to change some colors and get labels to pop up in the bar charts (compare with default files to see how I did this, deleting /node_modules/ and running "npm install" in terminal will load default /node_modules/ files). Can tweak further for customization and/or learn from this and other ONS-built chart types to build additional chart types
- /node_modules/@onsvisual/svelte-charts/src/charts/ScatterChart.svelte: Call when making a scatter chart, see example in App.svelte. Tweaked from default ScatterChart.svelte.
- 
- /src/layout/DataTeamHeader.svelte: Makes webiste header with Data Team text title and logo and links to Data Team homepage
- /src/layout/DataTeamFooter.svelte: Makes website footer with Data Team text title and logo, links to Data Team homepage, and provides Data Team email
- /src/layout/Header.svelte: Location to put title, authors, date. Probably don't need to change code here
- /src/layout/Filler.svelte: Differentiated section for text you want to bring attention to, e.g., intro/setup. See App.svelte for example. Probably don't need to change code here
- /src/layout/Media.svelte: Wrapper around figures, probably don't need to change code here
- 
- /public/index.html: Once built, this will be the html file for the website. Alter code here to change webpage tab text and description text when sharing the link. Also add google analytics tag here
