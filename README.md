const express = require("express");
2const app = express();
3// This is your real test secret API key.
4const stripe = require("stripe")("sk_test_51J1v7cE65bQBgfvu04vcdWuGiUoFxAhXbNIUI5IMORstlgpOrjCjZ0hTrtDSwTQoj5f3cETage25Mbgok4Ox8IVN005rD1hmqG");
5
6app.use(express.static("."));
7app.use(express.json());
8
9const calculateOrderAmount = items => {
10  // Replace this constant with a calculation of the order's amount
11  // Calculate the order total on the server to prevent
12  // people from directly manipulating the amount on the client
13  return 1400;
14};
15
16app.post("/create-payment-intent", async (req, res) => {
17  const { items } = req.body;
18  // Create a PaymentIntent with the order amount and currency
19  const paymentIntent = await stripe.paymentIntents.create({
20    amount: calculateOrderAmount(items),
21    currency: "usd"
22  });
23
24  res.send({
25    clientSecret: paymentIntent.client_secret
26  });
27});
28
29app.listen(4242, () => console.log('Node server listening on port 4242!'));
