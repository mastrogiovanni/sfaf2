<script lang="ts">
  import { Badge, Form, FormGroup, Input, Label } from "sveltestrap";
  import { Toast, ToastBody, ToastHeader } from "sveltestrap";
  import { Accordion, AccordionItem } from 'sveltestrap';
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

  let total = 0;
  let anticipo = 0;

  type CheckedMap = {
    [key: string]: boolean;
  };

  type NumberMap = {
    [key: string]: number;
  };

  let residenziali: CheckedMap = {}
  let bambiniResidenziali: CheckedMap = {}
  let bambiniEta: NumberMap = {}

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

  function computo(
    residenziali: CheckedMap,
    bambiniResidenziali: CheckedMap,
    bambiniEta: NumberMap
    ) {
    total = 0;
    anticipo = 0;
    for (let key in residenziali) {
      if (residenziali[key]) {
        total += 53
      } else {
        total += 75
      }
    }

    for (let key in bambiniResidenziali) {
      if (!bambiniResidenziali[key]) {
        total += 45
      } else {
        if (parseInt(key) < 2) {
          total += 40
        } else {
          total += 20
        }
      }
      if (bambiniEta[key] > 12) {
        total += 5
      }
    }

    console.log(total)
  }

  $: computo(
    residenziali,
    bambiniResidenziali,
    bambiniEta
  )

  const colors = [
    "primary",
    "secondary",
    "success",
    "danger",
    "warning",
    "info",
    "light",
    "dark",
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
</Card>

{#if total > 0}
  <div class="p-3 bg-primary mb-3">
    <Toast class="me-1">
      <ToastHeader>Totale</ToastHeader>
      <ToastBody>
        Il totale da versare è di {total}&euro;<br />
        L'anticipo è di {anticipo}&euro; da versare ad Alessandro a questo IBAN:<br
        />
        IT04Q0503403272000000011917
      </ToastBody>
    </Toast>
  </div>
{/if}

<Accordion>
  <AccordionItem header="Adulti">
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
              Residenziali 53&euro;<br />
              Non Residenzial 75&euro;
            </small>
          </CardBody>
          <!--
          <CardFooter><Badge>12&euro;</Badge></CardFooter>
          -->
        </Card>
    
        {#if adulti > 0}
            {#each adultiS as item, i}
            <Card class="mb-3">
              <CardBody color={colors[i]}>
                <FormGroup>
                  <Input
                    bind:checked={residenziali[item]}
                    onclick={() => { console.log(residenziali)}}
                    type="checkbox"
                    label="Adulto {i+1} Residenziale"
                  />
                </FormGroup>
              </CardBody>
              <CardFooter>
                <Badge>
                  {#if residenziali[item]}
                    53&euro;
                  {:else}
                    75&euro;
                  {/if}
              </Badge></CardFooter>
            </Card>
            {/each}
        {/if}
      </Form>
    </section>
  </AccordionItem>

  <AccordionItem header="Bambini">
    <section>
      <Form>
        <Card class="mb-3">
          <CardBody>
            <CardText>
              <FormGroup floating label="Numero Bambini">
                <Input bind:value={bambini} placeholder="Enter a value" />
              </FormGroup>
            </CardText>
            <small>
              Residenziali 0-2 anni 40&euro;<br />
              Residenziali &gt; 2 anni 40&euro;<br />
              Residenziali dal terzo figlio 20&euro;<br/>
              Non Residenzial &gt; 2 anni 45&euro;
            </small>
          </CardBody>
          <!--
          <CardFooter><Badge>12&euro;</Badge></CardFooter>
          -->
        </Card>
    
        {#if bambini > 0}
            {#each bambiniS as item, i}
            <Card class="mb-3">
              <CardBody color={colors[i]}>
                <FormGroup>
                  <Input bind:value={bambiniEta[item]} placeholder="Enter a value" />
                  <Input
                    bind:checked={bambiniResidenziali[item]}
                    onclick={() => { console.log(bambiniResidenziali)}}
                    type="checkbox"
                    label="Bambino {i+1} Residenziale"
                  />
                </FormGroup>
              </CardBody>
              <CardFooter>
                <Badge>
                  {#if bambiniResidenziali[item]}
                    {#if i < 2}
                      {#if bambiniEta[item] > 2}
                        40&euro;{#if bambiniEta[item] > 12}+ 5&euro; tassa di soggiorno{/if}
                      {:else}
                        40&euro;{#if bambiniEta[item] > 12}+ 5&euro; tassa di soggiorno{/if}
                      {/if} 
                    {:else}
                      20&euro;{#if bambiniEta[item] > 12}+ 5&euro; tassa di soggiorno{/if}
                    {/if}
                  {:else}
                    45&euro;{#if bambiniEta[item] > 12}+ 5&euro; tassa di soggiorno{/if}
                  {/if}
                </Badge></CardFooter>
            </Card>
            {/each}
        {/if}
      </Form>
    </section>
  </AccordionItem>

</Accordion>

<!--
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
