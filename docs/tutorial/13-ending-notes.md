import { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { motion } from "framer-motion";

const cardTypes = ["heart", "sword", "shield"];
const skills = ["swap", "steal", "flip", "destroy", "block"];

const generateCards = () => {
  const cards = [];
  for (let i = 0; i < 10; i++) {
    cards.push({
      id: `pt-${i}`,
      heart: Math.floor(Math.random() * 10) + 1,
      sword: Math.floor(Math.random() * 10) + 1,
      shield: Math.floor(Math.random() * 10) + 1,
      type: "point",
    });
  }
  const skillCards = skills
    .sort(() => 0.5 - Math.random())
    .slice(0, 3)
    .map((skill, i) => ({ id: `sk-${i}`, skill, type: "skill" }));
  return [...cards, ...skillCards];
};

export default function GameBoard() {
  const [playerHand, setPlayerHand] = useState([]);
  const [roundType, setRoundType] = useState(null);
  const [selectedCard, setSelectedCard] = useState(null);

  useEffect(() => {
    setPlayerHand(generateCards());
    setRoundType(cardTypes[Math.floor(Math.random() * cardTypes.length)]);
  }, []);

  const playCard = (card) => {
    if (selectedCard) return;
    setSelectedCard(card);
    // Normally would send to server
  };

  return (
    <div className="p-4 grid gap-4">
      <div className="text-xl font-bold text-center">
        รอบนี้เล่นแต้ม: <span className="capitalize">{roundType}</span>
      </div>

      <div className="grid grid-cols-3 gap-2">
        {playerHand.map((card) => (
          <motion.div
            key={card.id}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
            className="cursor-pointer"
            onClick={() => playCard(card)}
          >
            <Card className={`border-2 ${selectedCard?.id === card.id ? "border-blue-500" : "border-transparent"}`}>
              <CardContent className="p-2 text-center">
                {card.type === "point" ? (
                  <>
                    <div className="text-sm">❤️ {card.heart}</div>
                    <div className="text-sm">🗡️ {card.sword}</div>
                    <div className="text-sm">🛡️ {card.shield}</div>
                  </>
                ) : (
                  <div className="text-sm font-bold uppercase">Skill: {card.skill}</div>
                )}
              </CardContent>
            </Card>
          </motion.div>
        ))}
      </div>

      {selectedCard && (
        <div className="text-center text-green-600 font-semibold">
          คุณเลือกไพ่แล้ว: {selectedCard.type === "point" ? `${roundType} = ${selectedCard[roundType]}` : `Skill: ${selectedCard.skill}`}
        </div>
      )}

      <Button
        className="mt-4"
        onClick={() => {
          setSelectedCard(null);
          setRoundType(cardTypes[Math.floor(Math.random() * cardTypes.length)]);
        }}
      >
        เริ่มรอบใหม่
      </Button>
    </div>
  );
}
