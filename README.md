# 1import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

const questions = [
  {
    q: "데이트에서 가장 중요한 것은?",
    options: [
      { text: "분위기", type: "romantic" },
      { text: "재미", type: "fun" },
      { text: "안정감", type: "stable" },
    ],
  },
  {
    q: "연인이 연락이 없을 때 나는?",
    options: [
      { text: "기다린다", type: "stable" },
      { text: "먼저 연락한다", type: "romantic" },
      { text: "신경 안 쓴다", type: "fun" },
    ],
  },
  {
    q: "이상적인 데이트는?",
    options: [
      { text: "카페 데이트", type: "romantic" },
      { text: "액티비티", type: "fun" },
      { text: "집에서 휴식", type: "stable" },
    ],
  },
  {
    q: "연애에서 나는 어떤 스타일?",
    options: [
      { text: "표현 많이 함", type: "romantic" },
      { text: "자유로움", type: "fun" },
      { text: "책임감 있음", type: "stable" },
    ],
  },
  {
    q: "싸웠을 때 나는?",
    options: [
      { text: "바로 풀고 싶다", type: "romantic" },
      { text: "시간이 필요", type: "stable" },
      { text: "가볍게 넘김", type: "fun" },
    ],
  },
];

const results = {
  romantic: {
    title: "감성적인 연애 스타일",
    desc: "사랑을 표현하는 것을 중요하게 생각하고 감정 중심의 연애를 합니다.",
    match: "안정적인 스타일과 잘 맞아요 💕",
  },
  fun: {
    title: "자유로운 연애 스타일",
    desc: "재미와 자유를 중요하게 생각하며 부담 없는 연애를 선호합니다.",
    match: "감성적인 스타일과 잘 맞아요 🎉",
  },
  stable: {
    title: "안정적인 연애 스타일",
    desc: "책임감 있고 신뢰를 기반으로 한 연애를 중요하게 생각합니다.",
    match: "자유로운 스타일과 잘 맞아요 🌿",
  },
};

export default function LoveStyleQuiz() {
  const [step, setStep] = useState(0);
  const [answers, setAnswers] = useState([]);
  const [result, setResult] = useState(null);

  const handleAnswer = (type) => {
    const newAnswers = [...answers, type];
    setAnswers(newAnswers);

    if (step + 1 < questions.length) {
      setStep(step + 1);
    } else {
      const counts = { romantic: 0, fun: 0, stable: 0 };
      newAnswers.forEach((a) => counts[a]++);

      const final = Object.keys(counts).reduce((a, b) =>
        counts[a] > counts[b] ? a : b
      );

      setResult(results[final]);
    }
  };

  const reset = () => {
    setStep(0);
    setAnswers([]);
    setResult(null);
  };

  return (
    <div className="flex items-center justify-center min-h-screen bg-pink-100 p-4">
      <Card className="w-full max-w-md p-4">
        <CardContent>
          {!result ? (
            <div>
              <h2 className="text-xl font-bold mb-4">연애 스타일 테스트</h2>
              <p className="mb-4">{questions[step].q}</p>
              <div className="flex flex-col gap-2">
                {questions[step].options.map((opt, i) => (
                  <Button key={i} onClick={() => handleAnswer(opt.type)}>
                    {opt.text}
                  </Button>
                ))}
              </div>
            </div>
          ) : (
            <div className="text-center">
              <h2 className="text-2xl font-bold mb-2">{result.title}</h2>
              <p className="mb-2">{result.desc}</p>
              <p className="mb-4">{result.match}</p>
              <Button onClick={reset}>다시하기</Button>
            </div>
          )}
        </CardContent>
      </Card>
    </div>
  );
}
