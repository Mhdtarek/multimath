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

  import Multiclassic from "./games/multiclassic/multiclassicloader.svelte";

  let selectedGame = 0;
</script>

<main>
  <!--game menu-->
  {#if selectedGame === 0}
    <MaterialApp>
      <Container>
        <Row>
          <Col>
            <Card style="max-width:350px;">
              <img
                src="https://raw.githubusercontent.com/Mhdtarek/multigamesimages/main/MM.png"
                alt="background"
              />
              <CardTitle>MultiClassic</CardTitle>
              <CardSubtitle>enkelt att spela</CardSubtitle>
              <CardActions>
                <Button on:click={() => (selectedGame = 1)} text
                  >spela multiclassic
                </Button>
              </CardActions>
            </Card>
          </Col>
          <Col>
            <Card style="max-width:350px;">
              <img src="//picsum.photos/350" alt="background" />
              <CardTitle>Top western road trips</CardTitle>
              <CardSubtitle>1,000 miles of wonder</CardSubtitle>
              <CardActions>
                <Button text>Button</Button>
              </CardActions>
            </Card>
          </Col>
          <Col>
            <Card style="max-width:350px;">
              <img src="//picsum.photos/350" alt="background" />
              <CardTitle>Top western road trips</CardTitle>
              <CardSubtitle>1,000 miles of wonder</CardSubtitle>
              <CardActions>
                <Button text>Button</Button>
              </CardActions>
            </Card>
          </Col>
          <Col>
            <Card style="max-width:350px;">
              <img src="//picsum.photos/350" alt="background" />
              <CardTitle>Top western road trips</CardTitle>
              <CardSubtitle>1,000 miles of wonder</CardSubtitle>
              <CardActions>
                <Button text>Button</Button>
              </CardActions>
            </Card>
          </Col>
        </Row>
      </Container>
    </MaterialApp>
  {/if}
  <!--host the game -->
  {#if selectedGame === 1}
    <Multiclassic />
  {/if}
</main>

<style>
</style>
