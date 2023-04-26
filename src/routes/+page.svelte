<script lang="ts">
  import { Badge, Form, FormGroup, Input, Label } from "sveltestrap";
  import { Toast, ToastBody, ToastHeader } from "sveltestrap";
  import { Button } from "sveltestrap";
  import Counter from "./Counter.svelte";
  import welcome from "$lib/images/svelte-welcome.webp";
  import welcome_fallback from "$lib/images/svelte-welcome.png";
  import {
    Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardSubtitle,
    CardText,
    CardTitle,
  } from "sveltestrap";

  let bambini = 0;
  let adulti = 0;
  let bambiniS: string[] = [];
  let adultiS: string[] = [];

  $: {
    bambiniS = [];
    for (let i = 0; i < bambini; i++) {
      bambiniS.push("" + i);
    }
  }

  $: {
    adultiS = [];
    for (let i = 0; i < adulti; i++) {
      adultiS.push("" + i);
    }
  }

  const colors = [
    'primary',
    'secondary',
    'success',
    'danger',
    'warning',
    'info',
    'light',
    'dark'
  ];
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<Card class="mb-3">
  <CardHeader>
    <CardTitle>Prenotazione per la SFAF</CardTitle>
  </CardHeader>
  <CardBody>
    <CardSubtitle>20/26 Agosto 2023 - Cetraro</CardSubtitle>
	<!--
    <CardText>
      Some quick example text to build on the card title and make up the bulk of
      the card's content.
    </CardText>
    <Button>Button</Button>
	-->
  </CardBody>
  <CardFooter><Badge>12&euro;</Badge></CardFooter>
</Card>

<div class="p-3 bg-primary mb-3">
  <Toast class="me-1">
    <ToastHeader>Sveltestrap</ToastHeader>
    <ToastBody>
      This is a toast on a primary background — check it out!
    </ToastBody>
  </Toast>
</div>

<section>
  <Form>
    <Card class="mb-3">
      <CardBody>
        <CardText>
          <FormGroup floating label="Numero Adulti">
            <Input bind:value={adulti} placeholder="Enter a value" />
          </FormGroup>
        </CardText>
		<small>
			Residenziali 53&euro;<br/>
			Non Residenzial 75&euro;
		</small>
      </CardBody>
      <CardFooter><Badge>12&euro;</Badge></CardFooter>
    </Card>

    {#if adulti > 0}
	<Card class="mb-3">
        {#each adultiS as item, i}
		<CardBody color={colors[i]} >
			<FormGroup floating label="Età Adulto {item}">
				<Input placeholder="Enter a value" />
				<Input type="checkbox" label="Residenziale" />
			</FormGroup>
		</CardBody>
        {/each}
	</Card>
    {/if}

    <FormGroup floating label="Numero Bambini">
      <Input bind:value={bambini} placeholder="Enter a value" />
    </FormGroup>

    {#each bambiniS as item, i}
      <FormGroup floating label="Età Bimbo {item}">
        <Input placeholder="Enter a value" />
      </FormGroup>
    {/each}

    <FormGroup>
      <Input id="c1" type="checkbox" label="Check me out" />
    </FormGroup>
  </Form>

  <FormGroup floating label="Imposta comunale di soggiorno" />
</section>

<!--
<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
-->
