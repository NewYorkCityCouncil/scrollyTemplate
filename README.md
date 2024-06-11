# scrollyTemplate

Template borrowed from https://github.com/onsvisual/svelte-scrolly, look at documentation and demo there too for help (most code is inherited from that repository).

Other helpful links:
- https://svelte.dev/
- https://layercake.graphics/
- https://github.com/sveltejs/svelte-scroller

Directions:
- Recommend using Visual Studio Code (VS Code)
- Clone this repository to VS Code
- Open terminal window in VS Code
- Type "npm install" in terminal to install dependencies (/node_modules folder) (may need to install "npm" first)
- Type "npm run dev" in terminal---spits out a local link that shows the webpage as you work on it (save your work and it will update the webpage)
- Type "npm run build" in terminal when finished with webpage, this will compile it (compiled code in /public folder)  

File explanations:
- /src/App.svelte: This is the main code file. Some example code is currently in the file
- /src/layout/Scroller.svelte: Call when making a Scrolly section, see examples in App.svelte. Probably don't need to change code in this file, see https://github.com/sveltejs/svelte-scroller for details
- /src/layout/Table.svelte: Call when making a table. See example in App.svelte
- /node_modules/@onsvisual/svelte-charts/src/charts/BarChart.svelte: Call when making a bar chart, see examples in App.svelte. ONS made this file and files for other chart types (see /node_modules/@onsvisual/svelte-charts/src/charts/ folder). Can use as-is as a function with parameters that can be set, or can tweak code within this file (and/or the lower-level functions that it calls) for additional customization. Can also learn from these ONS-built chart types to build additional chart types
- /node_modules/@onsvisual/svelte-charts/src/charts/ScatterChart.svelte: Call when making a scatter chart, see example in App.svelte
- /src/layout/DataTeamHeader.svelte: Makes webiste header with Data Team text title and logo and links to Data Team homepage
- /src/layout/DataTeamFooter.svelte:
- /src/layout/Header.svelte:
- /src/layout/Filler.svelte:
- /src/layout/Section.svelte:
- /src/layout/Divider.svelte:
- /src/layout/Media.svelte:
- /public/index.html: Once built, this will be the html file for the website. Alter code here to change webpage tab text and description text when sharing the link. Also add google analytics tag here
