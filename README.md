import React, { useEffect, useMemo, useState } from "react";
import { motion } from "framer-motion";
import { Volume2, BookOpen, Star, CheckCircle2, RotateCcw, Globe2, Puzzle, GraduationCap } from "lucide-react";
import { Card, CardContent, CardHeader, CardTitle, CardDescription } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Progress } from "@/components/ui/progress";
import { Badge } from "@/components/ui/badge";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { Input } from "@/components/ui/input";

const lessons = [
  {
    id: 1,
    title: "Greetings",
    level: "Beginner",
    goal: "Say hello, introduce yourself, and ask simple questions.",
    tipEn: "Use short, clear sentences.",
    tipZh: "先学最常用的短句。",
    words: [
      { en: "Hello", zh: "你好", example: "Hello, I am Mei." },
      { en: "Hi", zh: "嗨", example: "Hi, how are you?" },
      { en: "My name is...", zh: "我的名字是……", example: "My name is Lily." },
      { en: "What is your name?", zh: "你叫什么名字？", example: "What is your name?" },
      { en: "Nice to meet you", zh: "很高兴见到你", example: "Nice to meet you, Anna." },
    ],
    mcq: [
      {
        q: "Which sentence is a greeting?",
        choices: ["Blue book.", "Hello!", "I have two cats.", "My sister is kind."],
        answer: "Hello!",
      },
      {
        q: "How do you ask somebody's name?",
        choices: ["What is your name?", "You are name?", "How name you?", "Name what?"],
        answer: "What is your name?",
      },
    ],
    blanks: [
      { sentence: "____, my name is Mei.", answer: "Hello", hint: "A common greeting." },
      { sentence: "What is your ____?", answer: "name", hint: "People call you by this." },
    ],
    builder: [
      { prompt: "Build the sentence", words: ["am", "I", "Lucy"], answer: "I am Lucy" },
      { prompt: "Build the sentence", words: ["your", "What", "is", "name"], answer: "What is your name" },
    ],
  },
  {
    id: 2,
    title: "Daily Life",
    level: "Beginner",
    goal: "Talk about simple everyday actions.",
    tipEn: "Practice with real daily routines.",
    tipZh: "把句子和每天的生活联系起来。",
    words: [
      { en: "eat", zh: "吃", example: "I eat breakfast." },
      { en: "drink", zh: "喝", example: "I drink tea." },
      { en: "go", zh: "去", example: "I go to the store." },
      { en: "cook", zh: "做饭", example: "I cook dinner." },
      { en: "sleep", zh: "睡觉", example: "I sleep at night." },
    ],
    mcq: [
      {
        q: "Which word is an action?",
        choices: ["mother", "eat", "blue", "name"],
        answer: "eat",
      },
      {
        q: "Which sentence is correct?",
        choices: ["I go to the store.", "I store go.", "Go I the store.", "Store I go."],
        answer: "I go to the store.",
      },
    ],
    blanks: [
      { sentence: "I ____ tea in the morning.", answer: "drink", hint: "You do this with water, tea, or coffee." },
      { sentence: "I ____ dinner in the evening.", answer: "cook", hint: "You do this in the kitchen." },
    ],
    builder: [
      { prompt: "Build the sentence", words: ["I", "breakfast", "eat"], answer: "I eat breakfast" },
      { prompt: "Build the sentence", words: ["go", "I", "home"], answer: "I go home" },
    ],
  },
  {
    id: 3,
    title: "Family",
    level: "Beginner",
    goal: "Describe family members using simple English.",
    tipEn: "Use 'My ... is ...' to describe people.",
    tipZh: "用 My ... is ... 来描述家人。",
    words: [
      { en: "mother", zh: "妈妈", example: "My mother is kind." },
      { en: "father", zh: "爸爸", example: "My father is tall." },
      { en: "sister", zh: "姐妹", example: "My sister is funny." },
      { en: "brother", zh: "兄弟", example: "My brother is strong." },
      { en: "family", zh: "家庭", example: "My family is happy." },
    ],
    mcq: [
      {
        q: "Which word is about family?",
        choices: ["family", "jump", "yellow", "drink"],
        answer: "family",
      },
      {
        q: "Which sentence is correct?",
        choices: ["My mother is kind.", "My kind mother is.", "Mother my is kind.", "Kind is my mother."],
        answer: "My mother is kind.",
      },
    ],
    blanks: [
      { sentence: "My ____ is my mother's daughter.", answer: "sister", hint: "A female sibling." },
      { sentence: "My ____ is happy.", answer: "family", hint: "This means all the people at home together." },
    ],
    builder: [
      { prompt: "Build the sentence", words: ["My", "kind", "mother", "is"], answer: "My mother is kind" },
      { prompt: "Build the sentence", words: ["family", "happy", "My", "is"], answer: "My family is happy" },
    ],
  },
  {
    id: 4,
    title: "Shopping and Food",
    level: "Beginner",
    goal: "Use English in stores and when talking about food.",
    tipEn: "Practice questions you can use outside the home.",
    tipZh: "学会在超市和点餐时常用的句子。",
    words: [
      { en: "price", zh: "价格", example: "What is the price?" },
      { en: "apple", zh: "苹果", example: "I want one apple." },
      { en: "rice", zh: "米饭", example: "I eat rice." },
      { en: "water", zh: "水", example: "Can I have water?" },
      { en: "How much?", zh: "多少钱？", example: "How much is this?" },
    ],
    mcq: [
      {
        q: "What do you say in a store?",
        choices: ["How much is this?", "My father is tall.", "The sky is blue.", "I sleep at night."],
        answer: "How much is this?",
      },
      {
        q: "Which word is food?",
        choices: ["rice", "family", "sleep", "teacher"],
        answer: "rice",
      },
    ],
    blanks: [
      { sentence: "How ____ is this?", answer: "much", hint: "You ask this for price." },
      { sentence: "Can I have some ____?", answer: "water", hint: "You drink this." },
    ],
    builder: [
      { prompt: "Build the sentence", words: ["this", "How", "much", "is"], answer: "How much is this" },
      { prompt: "Build the sentence", words: ["want", "I", "rice"], answer: "I want rice" },
    ],
  },
];

const lessonColors = [
  "from-blue-500 to-sky-400",
  "from-emerald-500 to-teal-400",
  "from-violet-500 to-fuchsia-400",
  "from-amber-500 to-orange-400",
];

function speak(text) {
  if (typeof window === "undefined" || !("speechSynthesis" in window)) return;
  const utterance = new SpeechSynthesisUtterance(text);
  utterance.lang = "en-US";
  utterance.rate = 0.92;
  window.speechSynthesis.cancel();
  window.speechSynthesis.speak(utterance);
}

function shuffle(arr) {
  const copy = [...arr];
  for (let i = copy.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [copy[i], copy[j]] = [copy[j], copy[i]];
  }
  return copy;
}

function normalize(text) {
  return String(text).trim().toLowerCase().replace(/[.!?]/g, "");
}

export default function EnglishLearningWebsite() {
  const [currentLesson, setCurrentLesson] = useState(0);
  const [showChinese, setShowChinese] = useState(true);
  const [stars, setStars] = useState(0);
  const [streak, setStreak] = useState(0);
  const [completedMap, setCompletedMap] = useState({});
  const [mcqIndex, setMcqIndex] = useState(0);
  const [blankIndex, setBlankIndex] = useState(0);
  const [builderIndex, setBuilderIndex] = useState(0);
  const [blankValue, setBlankValue] = useState("");
  const [blankFeedback, setBlankFeedback] = useState("Type the missing word.");
  const [mcqFeedback, setMcqFeedback] = useState("Choose the best answer.");
  const [builderFeedback, setBuilderFeedback] = useState("Tap the words in the correct order.");
  const [pickedWords, setPickedWords] = useState([]);
  const [builderChoices, setBuilderChoices] = useState([]);

  const lesson = lessons[currentLesson];
  const progress = useMemo(() => {
    return Math.round((Object.keys(completedMap).length / lessons.length) * 100);
  }, [completedMap]);

  useEffect(() => {
    const saved = localStorage.getItem("english-learning-site-data");
    if (saved) {
      try {
        const parsed = JSON.parse(saved);
        setStars(parsed.stars || 0);
        setStreak(parsed.streak || 0);
        setCompletedMap(parsed.completedMap || {});
        setShowChinese(parsed.showChinese ?? true);
        setCurrentLesson(parsed.currentLesson || 0);
      } catch {}
    }
  }, []);

  useEffect(() => {
    localStorage.setItem(
      "english-learning-site-data",
      JSON.stringify({ stars, streak, completedMap, showChinese, currentLesson })
    );
  }, [stars, streak, completedMap, showChinese, currentLesson]);

  useEffect(() => {
    resetMiniGames();
  }, [currentLesson]);

  useEffect(() => {
    setBuilderChoices(shuffle(lesson.builder[builderIndex].words));
    setPickedWords([]);
  }, [builderIndex, lesson]);

  function resetMiniGames() {
    setMcqIndex(0);
    setBlankIndex(0);
    setBuilderIndex(0);
    setBlankValue("");
    setBlankFeedback("Type the missing word.");
    setMcqFeedback("Choose the best answer.");
    setBuilderFeedback("Tap the words in the correct order.");
    setBuilderChoices(shuffle(lesson.builder[0].words));
    setPickedWords([]);
  }

  function reward(points) {
    setStars((s) => s + points);
    setStreak((s) => s + 1);
    setCompletedMap((prev) => ({ ...prev, [lesson.id]: true }));
  }

  function resetStreakOnly() {
    setStreak(0);
  }

  const currentMcq = lesson.mcq[mcqIndex];
  const currentBlank = lesson.blanks[blankIndex];
  const currentBuilder = lesson.builder[builderIndex];

  function handleMcq(choice) {
    if (choice === currentMcq.answer) {
      setMcqFeedback("Correct! Great job.");
      reward(2);
    } else {
      setMcqFeedback(`Not quite. Correct answer: ${currentMcq.answer}`);
      resetStreakOnly();
    }
  }

  function handleBlankCheck() {
    if (!blankValue.trim()) {
      setBlankFeedback("Please type an answer first.");
      return;
    }
    if (normalize(blankValue) === normalize(currentBlank.answer)) {
      setBlankFeedback("Correct! Well done.");
      reward(3);
    } else {
      setBlankFeedback(`Try again. Correct answer: ${currentBlank.answer}`);
      resetStreakOnly();
    }
  }

  function handlePickWord(word, idx) {
    setPickedWords((prev) => [...prev, { word, idx }]);
  }

  function resetBuilder() {
    setPickedWords([]);
    setBuilderFeedback("Tap the words in the correct order.");
    setBuilderChoices(shuffle(currentBuilder.words));
  }

  function checkBuilder() {
    const built = pickedWords.map((w) => w.word).join(" ");
    if (!built.trim()) {
      setBuilderFeedback("Choose some words first.");
      return;
    }
    if (normalize(built) === normalize(currentBuilder.answer)) {
      setBuilderFeedback("Excellent! The sentence is correct.");
      reward(4);
    } else {
      setBuilderFeedback(`Good try. Correct sentence: ${currentBuilder.answer}`);
      resetStreakOnly();
    }
  }

  const disabledIndexes = new Set(pickedWords.map((p) => `${p.word}-${p.idx}`));

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900">
      <section className="relative overflow-hidden bg-gradient-to-br from-slate-950 via-blue-950 to-sky-700 text-white">
        <div className="absolute inset-0 opacity-20 bg-[radial-gradient(circle_at_top_right,white,transparent_30%),radial-gradient(circle_at_bottom_left,white,transparent_25%)]" />
        <div className="relative mx-auto max-w-7xl px-6 py-12 md:px-8 md:py-16">
          <motion.div initial={{ opacity: 0, y: 16 }} animate={{ opacity: 1, y: 0 }} className="grid gap-8 lg:grid-cols-[1.25fr_0.9fr] lg:items-center">
            <div>
              <Badge className="mb-4 rounded-full bg-white/15 px-4 py-1 text-white hover:bg-white/15">Real beginner English website</Badge>
              <h1 className="max-w-3xl text-4xl font-bold tracking-tight md:text-5xl">
                English Learning Journey for Teaching a Beginner at Home
              </h1>
              <p className="mt-4 max-w-2xl text-base leading-7 text-blue-50 md:text-lg">
                A friendly website for helping your mom learn everyday English through short lessons,
                simple games, and clear sentence practice. Progress is saved automatically in the browser.
              </p>
              <div className="mt-6 flex flex-wrap gap-3">
                <Button size="lg" className="rounded-2xl bg-white text-slate-900 hover:bg-slate-100" onClick={() => document.getElementById("lesson-section")?.scrollIntoView({ behavior: "smooth" })}>
                  Start Learning
                </Button>
                <Button
                  size="lg"
                  variant="outline"
                  className="rounded-2xl border-white/30 bg-white/10 text-white hover:bg-white/15"
                  onClick={() => setShowChinese((v) => !v)}
                >
                  <Globe2 className="mr-2 h-4 w-4" />
                  {showChinese ? "Hide Chinese Help" : "Show Chinese Help"}
                </Button>
              </div>
            </div>

            <Card className="rounded-3xl border-white/10 bg-white/10 text-white shadow-2xl backdrop-blur">
              <CardHeader>
                <CardTitle className="text-2xl">Learning Dashboard</CardTitle>
                <CardDescription className="text-blue-100">Simple tracking for practice at home</CardDescription>
              </CardHeader>
              <CardContent className="grid gap-4 sm:grid-cols-2">
                <div className="rounded-2xl bg-white/10 p-4">
                  <div className="text-sm text-blue-100">Progress</div>
                  <div className="mt-1 text-3xl font-bold">{progress}%</div>
                </div>
                <div className="rounded-2xl bg-white/10 p-4">
                  <div className="text-sm text-blue-100">Stars</div>
                  <div className="mt-1 text-3xl font-bold">{stars}</div>
                </div>
                <div className="rounded-2xl bg-white/10 p-4">
                  <div className="text-sm text-blue-100">Current lesson</div>
                  <div className="mt-1 text-3xl font-bold">{lesson.id}</div>
                </div>
                <div className="rounded-2xl bg-white/10 p-4">
                  <div className="text-sm text-blue-100">Streak</div>
                  <div className="mt-1 text-3xl font-bold">{streak}</div>
                </div>
              </CardContent>
            </Card>
          </motion.div>
        </div>
      </section>

      <main className="mx-auto max-w-7xl px-6 py-8 md:px-8 md:py-10">
        <section className="grid gap-4 md:grid-cols-4">
          {[
            { icon: BookOpen, title: "Lesson path", text: "Clear beginner topics with practical English." },
            { icon: Puzzle, title: "Mini games", text: "Fill in the blank, quizzes, and sentence building." },
            { icon: Volume2, title: "Speak aloud", text: "Play English words and sample phrases out loud." },
            { icon: GraduationCap, title: "Home teaching", text: "Useful for teaching a parent or adult beginner." },
          ].map((item, i) => (
            <motion.div key={item.title} initial={{ opacity: 0, y: 16 }} animate={{ opacity: 1, y: 0 }} transition={{ delay: i * 0.05 }}>
              <Card className="h-full rounded-3xl border-slate-200 shadow-sm">
                <CardContent className="p-6">
                  <item.icon className="mb-3 h-8 w-8 text-blue-600" />
                  <h3 className="text-lg font-semibold">{item.title}</h3>
                  <p className="mt-2 text-sm leading-6 text-slate-600">{item.text}</p>
                </CardContent>
              </Card>
            </motion.div>
          ))}
        </section>

        <section id="lesson-section" className="mt-8 grid gap-8 lg:grid-cols-[320px_minmax(0,1fr)]">
          <aside className="space-y-4">
            <Card className="rounded-3xl border-slate-200 shadow-sm">
              <CardHeader>
                <CardTitle>Course Roadmap</CardTitle>
                <CardDescription>Choose a lesson and practice in any order</CardDescription>
              </CardHeader>
              <CardContent className="space-y-3">
                {lessons.map((item, idx) => {
                  const active = idx === currentLesson;
                  const complete = Boolean(completedMap[item.id]);
                  return (
                    <button
                      key={item.id}
                      onClick={() => setCurrentLesson(idx)}
                      className={`w-full rounded-2xl border p-4 text-left transition ${
                        active ? "border-blue-500 bg-blue-50 shadow-sm" : "border-slate-200 bg-white hover:border-slate-300"
                      }`}
                    >
                      <div className="flex items-start justify-between gap-3">
                        <div>
                          <div className="text-sm font-semibold text-slate-500">Lesson {item.id}</div>
                          <div className="mt-1 font-semibold">{item.title}</div>
                          <div className="mt-1 text-sm text-slate-600">{item.goal}</div>
                        </div>
                        {complete ? <CheckCircle2 className="mt-1 h-5 w-5 text-emerald-600" /> : <Star className="mt-1 h-5 w-5 text-amber-500" />}
                      </div>
                    </button>
                  );
                })}
                <div className="pt-2">
                  <div className="mb-2 flex items-center justify-between text-sm text-slate-600">
                    <span>Overall progress</span>
                    <span>{progress}%</span>
                  </div>
                  <Progress value={progress} className="h-3 rounded-full" />
                </div>
              </CardContent>
            </Card>

            <Card className="rounded-3xl border-slate-200 shadow-sm">
              <CardHeader>
                <CardTitle>Teaching Tip</CardTitle>
                <CardDescription>Use the lesson slowly and repeat aloud</CardDescription>
              </CardHeader>
              <CardContent className="space-y-3 text-sm leading-6 text-slate-700">
                <p><span className="font-semibold">English:</span> {lesson.tipEn}</p>
                {showChinese && <p><span className="font-semibold">中文:</span> {lesson.tipZh}</p>}
                <Button variant="outline" className="w-full rounded-2xl" onClick={() => { localStorage.removeItem("english-learning-site-data"); setStars(0); setStreak(0); setCompletedMap({}); }}>
                  <RotateCcw className="mr-2 h-4 w-4" />
                  Reset Saved Progress
                </Button>
              </CardContent>
            </Card>
          </aside>

          <div className="space-y-6">
            <motion.div key={lesson.id} initial={{ opacity: 0, y: 12 }} animate={{ opacity: 1, y: 0 }}>
              <Card className="overflow-hidden rounded-3xl border-slate-200 shadow-sm">
                <div className={`bg-gradient-to-r ${lessonColors[(lesson.id - 1) % lessonColors.length]} p-6 text-white`}>
                  <div className="flex flex-wrap items-center justify-between gap-4">
                    <div>
                      <div className="text-sm font-semibold uppercase tracking-wide text-white/80">{lesson.level}</div>
                      <h2 className="mt-1 text-3xl font-bold">{lesson.title}</h2>
                      <p className="mt-2 max-w-2xl text-white/90">{lesson.goal}</p>
                    </div>
                    <Badge className="rounded-full bg-white/20 px-4 py-1 text-white hover:bg-white/20">Lesson {lesson.id}</Badge>
                  </div>
                </div>
                <CardContent className="p-6">
                  <Tabs defaultValue="words" className="w-full">
                    <TabsList className="grid w-full grid-cols-4 rounded-2xl bg-slate-100 p-1">
                      <TabsTrigger value="words" className="rounded-xl">Words</TabsTrigger>
                      <TabsTrigger value="quiz" className="rounded-xl">Quiz</TabsTrigger>
                      <TabsTrigger value="blank" className="rounded-xl">Fill Blank</TabsTrigger>
                      <TabsTrigger value="builder" className="rounded-xl">Build Sentence</TabsTrigger>
                    </TabsList>

                    <TabsContent value="words" className="mt-6">
                      <div className="grid gap-4 md:grid-cols-2 xl:grid-cols-3">
                        {lesson.words.map((word) => (
                          <Card key={word.en} className="rounded-2xl border-slate-200 shadow-none">
                            <CardContent className="p-5">
                              <div className="flex items-start justify-between gap-3">
                                <div>
                                  <div className="text-xl font-bold">{word.en}</div>
                                  {showChinese && <div className="mt-1 text-sm text-slate-500">{word.zh}</div>}
                                </div>
                                <Button size="icon" variant="outline" className="rounded-full" onClick={() => speak(word.en)}>
                                  <Volume2 className="h-4 w-4" />
                                </Button>
                              </div>
                              <p className="mt-4 text-sm leading-6 text-slate-600">{word.example}</p>
                            </CardContent>
                          </Card>
                        ))}
                      </div>
                    </TabsContent>

                    <TabsContent value="quiz" className="mt-6">
                      <Card className="rounded-2xl border-slate-200 shadow-none">
                        <CardContent className="p-6">
                          <div className="mb-4 flex items-center justify-between gap-3">
                            <div>
                              <div className="text-sm text-slate-500">Question {mcqIndex + 1} / {lesson.mcq.length}</div>
                              <h3 className="mt-1 text-xl font-semibold">{currentMcq.q}</h3>
                            </div>
                            <Button variant="outline" className="rounded-2xl" onClick={() => setMcqIndex((v) => (v + 1) % lesson.mcq.length)}>
                              Next
                            </Button>
                          </div>
                          <div className="grid gap-3 md:grid-cols-2">
                            {currentMcq.choices.map((choice) => (
                              <Button
                                key={choice}
                                variant="outline"
                                className="h-auto min-h-14 justify-start rounded-2xl px-4 py-3 text-left whitespace-normal"
                                onClick={() => handleMcq(choice)}
                              >
                                {choice}
                              </Button>
                            ))}
                          </div>
                          <div className="mt-4 rounded-2xl bg-slate-50 p-4 text-sm text-slate-700">{mcqFeedback}</div>
                        </CardContent>
                      </Card>
                    </TabsContent>

                    <TabsContent value="blank" className="mt-6">
                      <Card className="rounded-2xl border-slate-200 shadow-none">
                        <CardContent className="p-6">
                          <div className="mb-4 flex items-center justify-between gap-3">
                            <div>
                              <div className="text-sm text-slate-500">Blank {blankIndex + 1} / {lesson.blanks.length}</div>
                              <h3 className="mt-1 text-xl font-semibold">{currentBlank.sentence}</h3>
                            </div>
                            <Button variant="outline" className="rounded-2xl" onClick={() => { setBlankIndex((v) => (v + 1) % lesson.blanks.length); setBlankValue(""); setBlankFeedback("Type the missing word."); }}>
                              Next
                            </Button>
                          </div>
                          <div className="flex flex-col gap-3 md:flex-row">
                            <Input
                              value={blankValue}
                              onChange={(e) => setBlankValue(e.target.value)}
                              placeholder="Type your answer"
                              className="h-12 rounded-2xl"
                              onKeyDown={(e) => e.key === "Enter" && handleBlankCheck()}
                            />
                            <Button className="h-12 rounded-2xl" onClick={handleBlankCheck}>Check</Button>
                            <Button variant="outline" className="h-12 rounded-2xl" onClick={() => setBlankFeedback(`Hint: ${currentBlank.hint}`)}>Hint</Button>
                          </div>
                          <div className="mt-4 rounded-2xl bg-slate-50 p-4 text-sm text-slate-700">{blankFeedback}</div>
                        </CardContent>
                      </Card>
                    </TabsContent>

                    <TabsContent value="builder" className="mt-6">
                      <Card className="rounded-2xl border-slate-200 shadow-none">
                        <CardContent className="p-6">
                          <div className="mb-4 flex items-center justify-between gap-3">
                            <div>
                              <div className="text-sm text-slate-500">Sentence {builderIndex + 1} / {lesson.builder.length}</div>
                              <h3 className="mt-1 text-xl font-semibold">{currentBuilder.prompt}</h3>
                            </div>
                            <Button variant="outline" className="rounded-2xl" onClick={() => setBuilderIndex((v) => (v + 1) % lesson.builder.length)}>
                              Next
                            </Button>
                          </div>

                          <div className="mb-4 min-h-16 rounded-2xl border border-dashed border-slate-300 bg-slate-50 p-4">
                            <div className="flex flex-wrap gap-2">
                              {pickedWords.length ? (
                                pickedWords.map((item, idx) => (
                                  <Badge key={`${item.word}-${idx}`} className="rounded-full bg-blue-100 px-3 py-1 text-blue-800 hover:bg-blue-100">{item.word}</Badge>
                                ))
                              ) : (
                                <span className="text-sm text-slate-500">Your sentence will appear here.</span>
                              )}
                            </div>
                          </div>

                          <div className="grid gap-3 md:grid-cols-2 xl:grid-cols-3">
                            {builderChoices.map((word, idx) => {
                              const key = `${word}-${idx}`;
                              const disabled = disabledIndexes.has(key);
                              return (
                                <Button
                                  key={key}
                                  variant="outline"
                                  className="justify-start rounded-2xl"
                                  disabled={disabled}
                                  onClick={() => handlePickWord(word, idx)}
                                >
                                  {word}
                                </Button>
                              );
                            })}
                          </div>

                          <div className="mt-4 flex flex-wrap gap-3">
                            <Button className="rounded-2xl" onClick={checkBuilder}>Check Sentence</Button>
                            <Button variant="outline" className="rounded-2xl" onClick={resetBuilder}>Reset</Button>
                          </div>
                          <div className="mt-4 rounded-2xl bg-slate-50 p-4 text-sm text-slate-700">{builderFeedback}</div>
                        </CardContent>
                      </Card>
                    </TabsContent>
                  </Tabs>
                </CardContent>
              </Card>
            </motion.div>

            <section className="grid gap-4 md:grid-cols-3">
              <Card className="rounded-3xl border-slate-200 shadow-sm md:col-span-2">
                <CardHeader>
                  <CardTitle>Useful Phrases for Your Mom</CardTitle>
                  <CardDescription>Short English for everyday situations</CardDescription>
                </CardHeader>
                <CardContent className="grid gap-3 sm:grid-cols-2">
                  {[
                    ["How are you?", "你好吗？"],
                    ["I am fine.", "我很好。"],
                    ["Please help me.", "请帮帮我。"],
                    ["Thank you.", "谢谢你。"],
                    ["Where is the washroom?", "洗手间在哪里？"],
                    ["How much is this?", "这个多少钱？"],
                  ].map(([en, zh]) => (
                    <div key={en} className="rounded-2xl border border-slate-200 p-4">
                      <div className="flex items-start justify-between gap-3">
                        <div>
                          <div className="font-semibold">{en}</div>
                          {showChinese && <div className="mt-1 text-sm text-slate-500">{zh}</div>}
                        </div>
                        <Button size="icon" variant="ghost" className="rounded-full" onClick={() => speak(en)}>
                          <Volume2 className="h-4 w-4" />
                        </Button>
                      </div>
                    </div>
                  ))}
                </CardContent>
              </Card>

              <Card className="rounded-3xl border-slate-200 shadow-sm">
                <CardHeader>
                  <CardTitle>How to Teach</CardTitle>
                  <CardDescription>Simple routine for a 10–15 minute lesson</CardDescription>
                </CardHeader>
                <CardContent className="space-y-3 text-sm leading-6 text-slate-700">
                  <div className="rounded-2xl bg-slate-50 p-4">1. Read 3 to 5 words aloud together.</div>
                  <div className="rounded-2xl bg-slate-50 p-4">2. Let her repeat each phrase slowly.</div>
                  <div className="rounded-2xl bg-slate-50 p-4">3. Do one quiz and one fill-in-the-blank.</div>
                  <div className="rounded-2xl bg-slate-50 p-4">4. End by saying two useful sentences from memory.</div>
                </CardContent>
              </Card>
            </section>
          </div>
        </section>
      </main>
    </div>
  );
}
