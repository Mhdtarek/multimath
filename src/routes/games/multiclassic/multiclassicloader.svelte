<script>
  import {
    Container,
    Row,
    Col,
    Card,
    CardTitle,
    CardSubtitle,
    CardActions,
    Button,
    Icon,
    Divider,
    MaterialApp,
  } from "svelte-materialify";
  import MultiMath from "./MultimathHost.svelte";

  let selectedGame = 0;
</script>

<main>
  {#if selectedGame == 0}
    <MaterialApp>
      <Container>
        <Row>
          <Col>
            <Card style="max-width:350px;">
              <img
                src="https://raw.githubusercontent.com/Mhdtarek/multigamesimages/main/MM.png"
                alt="background"
              />
              <CardTitle>multimath</CardTitle>
              <CardSubtitle>Kul matte spel</CardSubtitle>
              <CardActions>
                <Button on:click={() => (selectedGame = 1)} text
                  >spela multimath
                </Button>
              </CardActions>
            </Card>
          </Col>
        </Row>
      </Container>
    </MaterialApp>
  {/if}
  {#if selectedGame === 1}
    <MultiMath />
  {/if}
</main>
