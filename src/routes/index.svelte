<script>
  let name;
  let email;
  let message;

  let processing = false;
  let response;
  let outcome = { status: '', message: '' };

  async function handleSubmit() {
    processing = true;

    try {
      const submit = await fetch('/api/contact', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          name,
          email,
          message,
        }),
      });

      response = await submit.json();
      outcome = response;

      name = '';
      email = '';
      message = '';
    } catch (error) {
      outcome.status = 500;
      outcome.message = 'Server failed to respond.';
      console.log(error)
    }

    processing = false;
  }
</script>

<svelte:head>
  <title>Contact</title>
</svelte:head>

{#if processing}
  Sending your message
{/if}

{#if outcome.status === 200}
  Your message was sent successfully.
{/if}

{#if outcome.status === 500}
    <p><strong>{outcome.message}</strong></p>
    There was a problem sending your message, please try again.<br />If the problem perists please email
    info@jace.info.
{/if}

<form on:submit|preventDefault={handleSubmit}>
  <div>
    <label for="name"> Your name (required) </label>
    <input id="name" type="text" required bind:value={name} />
  </div>

  <div>
    <label for="email">Your email address (required)</label>
    <input id="email" type="email" required bind:value={email} />
  </div>

  <div>
    <label for="message">Your message (required)</label>
    <textarea id="message"  rows={5} required bind:value={message} />
  </div>

  <input class="button submit" type="submit" value="Send your message" />
</form>


