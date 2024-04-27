# RAW JSON DATA

## Application with the raw json data

```js
$(document).ready(function() {
    
    document.getElementById('loadDataSetBtn').addEventListener('click', loadDataSet);
    document.getElementById('downloadBtn').addEventListener('click', downloadAsJson);
    let jsonFile = './jsons/coding_languages.json';

    function loadDataSet () {
        jsonFile = document.getElementById('dataset').value;
        jsonFile = `./jsons/${jsonFile}`;
        renderJson();
    }

    /*Download as .json file*/
    function downloadAsJson() {
        const a = document.createElement("a");
        const content = document.getElementById('rawJson').value;
        const file = new Blob([content], { type: 'text/plain' });
        a.href = URL.createObjectURL(file);
        a.download = document.getElementById('dataset').value;
        a.click();
    }

    const renderJson = async () => {
        await fetch(jsonFile)
        .then((response) => {
            return response.json();
        }).then((data) => {
            let options = {
                collapsed: document.getElementById("collapsed").checked,
                rootCollapsable: document.getElementById("root-collapsable").checked,
                withQuotes: document.getElementById("with-quotes").checked,
                withLinks: document.getElementById("with-links").checked
            };
            $('#json-renderer').jsonViewer(data, options);
            document.getElementById('rawJson').value = JSON.stringify(data);
        }).catch((error) => {
            console.log(error);
        });
    };

    /*Generate on option change*/
    $('p.options input[type=checkbox]').click(renderJson);

    /*Display JSON sample on page load*/
    renderJson();
});
```