---
layout: default
title: List of repositories
has_children: true
nav_order: 2
---

# List of repositories
Here you can find a list of **community maintained** extension repositories. **We are not affiliated with any of them.**.
You can install any repository, by clicking the <kbd>Install</kbd> button. If that did not work for you, check out [how to install repositories manually](./how-to-install-manually.md).

<div id="main">

Loading data...

If you're using GitHub, please visit this page through [the website](https://recloudstream.github.io) as it loads content dynamically.

</div>

<!-- new lines get replaced with spaces, so remember about ; -->
<script>
function makeTd(text) {
    const td = document.createElement("td");
    td.appendChild(
        document.createTextNode(text)
    );
    return td;
}

async function fetchRow(url) {
    const r = await fetch(url);
    const data = await r.json();
    const row = document.createElement("tr");
    row.appendChild(makeTd(data.name || "unnamed"));
    row.appendChild(makeTd(data.description || "No description provided"));

    const td = document.createElement("td");
    td.style.display = 'flex';
    
    const btn1 = document.createElement("a");
    btn1.innerText = "Install";
    btn1.target = "_blank";
    btn1.classList.add("btn");
    btn1.classList.add("btn-blue");
    btn1.href = `https://cs.repo/?${url.replace(/^https?:\/\//, "")}`;
    td.appendChild(btn1);

    const btn2 = document.createElement("button");
    btn2.innerText = "Copy URL";
    btn2.classList.add("btn");
    btn2.addEventListener("click", () => {
        if (navigator.clipboard) {
            navigator.clipboard.writeText(url);
        } else {
            var tempInput = document.createElement("input");
            tempInput.value = url;
            document.body.appendChild(tempInput);
            tempInput.select();
            document.execCommand("copy");
            document.body.removeChild(tempInput);
        }
    });
    td.appendChild(btn2);
    
    row.appendChild(td);
    return row;
}

async function fetchData() {
    const mainDiv = document.getElementById("main");
    
    const table = document.createElement("table");
    const thead = document.createElement("thead");
    thead.innerHTML = `
    <tr>
        <th>Name</th>
        <th>Description</th>
        <th>Install</th>
    </tr>
`;
    table.appendChild(thead);

    const tbody = document.createElement("tbody");
    const r = await fetch("https://raw.githubusercontent.com/recloudstream/cs-repos/master/repos-db.json");
    const data = await r.json();

    for (const repo of data) {
        const row = await(fetchRow(repo));
        tbody.appendChild(row);
    }
    table.appendChild(tbody);

    mainDiv.innerHTML = '';
    mainDiv.classList.add("table-wrapper");
    mainDiv.appendChild(table);
}

fetchData();
</script>
