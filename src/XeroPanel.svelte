<script>
    import { getContext } from "svelte";
    import { createEventDispatcher } from "svelte";
    const sdk = getContext("sdk");
    const dispatcher = createEventDispatcher();

    export let text;

    let matches = window.location.href.match("\\?(.*?)(?=#)");
    if (matches && matches[1]) {
        text = matches[1];
    } else {
        console.log("No match found or undefined");
    }
    function update_access_token() {
        dispatcher("before_submit",text);
        sdk.API.getDatasources().then((value) => {
            value.forEach((item) => {
                let ds_name = item.name.toString().toLowerCase();
                if (ds_name.includes("xero")) {
                    item.config.key = text;
                    sdk.API.updateDatasource(item);
                }
            });
            dispatcher("after_submit",text);
            text = "";
        });
    }
</script>

<div class="input-container">
    <label for="name"
        >Xero - Logged in Successfully - Click the button to finish the process.</label
    >
    <input
        type="text"
        id="name"
        name="name"
        placeholder="Oath Token URL Here"
        bind:value={text}
    />
    <input type="submit" value="Submit" on:click={update_access_token} />
</div>

<style>
    .input-container {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    label {
        font-size: 18px;
        color: #333;
    }

    input[type="text"] {
        padding: 10px;
        width: 300px;
        border: 2px solid #ccc;
        border-radius: 4px;
        font-size: 16px;
        transition: border-color 0.3s ease;
    }

    input[type="text"]:focus {
        outline: none;
        border-color: #007bff; /* Blue border on focus */
        box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
    }

    input[type="submit"] {
        padding: 10px 15px;
        background-color: #007bff;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 16px;
        transition: background-color 0.3s ease;
    }

    input[type="submit"]:hover {
        background-color: #0056b3; /* Darker blue on hover */
    }
</style>
