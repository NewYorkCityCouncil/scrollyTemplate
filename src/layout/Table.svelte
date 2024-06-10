<script>
    // code adapted from https://svelte.dev/repl/7b02f45e49744502bf5f03cb61375f9f?version=4.2.8
    import Title from "@onsvisual/svelte-charts/src/charts/shared/Title.svelte";
    import Footer from "@onsvisual/svelte-charts/src/charts/shared/Footer.svelte";

    export let tableData = [
        {
            vitae: "dolorem",
            lectus: "ipsum",
            quisquam: "quia",
        },
        {
            vitae: "amet",
            lectus: "consectetur",
            quisquam: "adipisci",
        },
    ];
    export let title = null;
    export let footer = null;
    export let style = "council";
</script>

{#if title}
    <Title>{title}</Title>
{/if}
<div class='table-container'>
<table class={style}>
    <thead>
        <tr>
            {#each Object.keys(tableData[0]) as columnHeading}
                <th>{columnHeading}</th>
            {/each}
        </tr><tr />
    </thead>
    <tbody>
        {#each Object.values(tableData) as row}
            <tr>
                {#each Object.values(row) as cell}
                    <td>{cell}</td>
                {/each}
            </tr>
        {/each}
    </tbody>
</table>
</div>
{#if footer}
    <Footer>{footer}</Footer>
{/if}

<style>
    table.council {
        border: 1px solid #fefefe;
        background-color: #fff;
        color: #000;
        width: 100%;
        border-collapse: collapse;
       
    }
    table.council thead {
        background: #f2ede9;
        white-space: nowrap;
    }
    table.council tr {
        border-bottom: 1px solid #bcbcbc;
    }
    table.council th {
        padding: 10px;
    }
    table.council td {
        padding-top: 5px;
        padding-bottom: 5px;
    }
    table.council tr:last-child {
        border-bottom: 1px solid #fefefe;
    }
    table.council thead tr th:first-child {
        font-size: 16px;
        font-weight: bold;
        text-align: left;
        font-family: Georgia;
    }
    table.council thead tr th:not(:first-child) {
        font-size: 16px;
        font-weight: bold;
        text-align: center;
        font-family: Georgia;
    }
    table.council tbody tr td:first-child {
        font-size: 14px;
        text-align: left;
        font-family: Georgia;
    }
    table.council tbody tr td:not(:first-child) {
        font-size: 14px;
        text-align: center;
        font-family: Georgia;
    }
     .table-container{
        overflow-x: auto;
    }
</style>
