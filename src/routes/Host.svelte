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
  import { slide } from "svelte/transition";
  import { mdiChevronDown } from "@mdi/js";
</script>

<main>
  <MaterialApp>
    <Container>
      <Row>
        <Col>
          <Card style="max-width:350px;">
            <img src="" alt="background" />
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
</main>

<style>
</style>
