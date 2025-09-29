# java.html.4.2
// server.js
const express = require("express");
const app = express();
const PORT = 3000;

// Middleware to parse JSON
app.use(express.json());

// In-memory collection of playing cards
let cards = [
  { id: 1, suit: "Hearts", value: "Ace" },
  { id: 2, suit: "Spades", value: "King" },
];

// Home route
app.get("/", (req, res) => {
  res.send("Welcome to the Playing Cards API!");
});

// GET all cards
app.get("/cards", (req, res) => {
  res.json(cards);
});

// GET a single card by ID
app.get("/cards/:id", (req, res) => {
  const cardId = parseInt(req.params.id);
  const card = cards.find(c => c.id === cardId);
  if (!card) return res.status(404).json({ message: "Card not found" });
  res.json(card);
});

// POST a new card
app.post("/cards", (req, res) => {
  const { suit, value } = req.body;
  if (!suit || !value) {
    return res.status(400).json({ message: "Suit and value are required" });
  }
  const newCard = {
    id: cards.length ? cards[cards.length - 1].id + 1 : 1,
    suit,
    value,
  };
  cards.push(newCard);
  res.status(201).json(newCard);
});

// PUT update a card
app.put("/cards/:id", (req, res) => {
  const cardId = parseInt(req.params.id);
  const card = cards.find(c => c.id === cardId);
  if (!card) return res.status(404).json({ message: "Card not found" });

  const { suit, value } = req.body;
  if (suit) card.suit = suit;
  if (value) card.value = value;

  res.json(card);
});

// DELETE a card
app.delete("/cards/:id", (req, res) => {
  const cardId = parseInt(req.params.id);
  const cardIndex = cards.findIndex(c => c.id === cardId);
  if (cardIndex === -1) return res.status(404).json({ message: "Card not found" });

  const deletedCard = cards.splice(cardIndex, 1);
  res.json(deletedCard[0]);
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
