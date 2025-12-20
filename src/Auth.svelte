<script>
  async function callApi() {
    const response = await fetch("http://server.mosanie.space:19090/api/auth/auth-with-sup", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({ token: "هنا ضعي التوكن الحقيقي" })
    });

    const data = await response.json();
    console.log(data);
  }
</script>

<button on:click={callApi}>Call API</button>