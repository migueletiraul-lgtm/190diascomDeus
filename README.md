import { useState } from "react";
import { ChevronLeft, ChevronRight, BookOpen, Grid3X3 } from "lucide-react";
import heroImage from "@/assets/hero-sunrise.jpg";
import DevotionalCard from "@/components/DevotionalCard";
import DaySelector from "@/components/DaySelector";
import ProgressBar from "@/components/ProgressBar";
import { getDevotionalByDay, getAvailableDays } from "@/data/devotionals";
import { useReadDays } from "@/hooks/useReadDays";

const Index = () => {
  const [currentDay, setCurrentDay] = useState(1);
  const [showSelector, setShowSelector] = useState(false);

  const available = getAvailableDays();
  const devotional = getDevotionalByDay(currentDay);
  const { readCount, toggleDay, isRead } = useReadDays();

  const goToPrev = () => setCurrentDay((d) => Math.max(1, d - 1));
  const goToNext = () => setCurrentDay((d) => Math.min(available, d + 1));

  return (
    <div className="min-h-screen bg-background">
      {/* HERO */}
      <header className="relative h-[45vh] min-h-[300px] flex items-center justify-center overflow-hidden">
        <img
          src={heroImage}
          alt="Nascer do sol sobre montanhas"
          className="absolute inset-0 w-full h-full object-cover"
        />

        <div className="absolute inset-0 bg-gradient-to-b from-black/50 via-black/30 to-background" />

        <div className="relative z-10 text-center px-6">
          <div className="flex items-center justify-center gap-2 mb-3">
            <span className="h-px w-8 bg-white/40" />
            <BookOpen className="h-5 w-5 text-white/80 animate-pulse" />
            <span className="h-px w-8 bg-white/40" />
          </div>

          <h1 className="text-3xl md:text-5xl lg:text-6xl font-bold text-white mb-3 drop-shadow-lg tracking-wide">
            190 Dias com Deus
          </h1>

          <p className="text-white/80 text-sm md:text-base max-w-md mx-auto">
            Uma jornada de fé, reflexão e transformação espiritual
          </p>
        </div>
      </header>

      {/* PROGRESSO + NAV */}
      <section className="py-6 px-4">
        <ProgressBar currentDay={currentDay} readCount={readCount} />

        <div className="flex items-center justify-center gap-3 mt-5">
          <button
            onClick={goToPrev}
            disabled={currentDay === 1}
            className="p-2.5 rounded-full border hover:bg-primary disabled:opacity-30"
          >
            <ChevronLeft className="h-4 w-4" />
          </button>

          <button
            onClick={() => setShowSelector(!showSelector)}
            className="flex items-center gap-2 px-5 py-2.5 rounded-full border hover:bg-primary/10 text-xs uppercase"
          >
            <Grid3X3 className="h-4 w-4" />
            Dia {currentDay}
          </button>

          <button
            onClick={goToNext}
            disabled={currentDay === available}
            className="p-2.5 rounded-full border hover:bg-primary disabled:opacity-30"
          >
            <ChevronRight className="h-4 w-4" />
          </button>
        </div>

        {showSelector && (
          <div className="mt-5">
            <DaySelector
              currentDay={currentDay}
              isRead={isRead}
              onSelectDay={(d) => {
                setCurrentDay(d);
                setShowSelector(false);
              }}
            />
          </div>
        )}
      </section>

      {/* CONTEÚDO */}
      <main className="px-4 pb-16">
        {devotional ? (
          <DevotionalCard
            key={currentDay}
            devotional={devotional}
            isRead={isRead(currentDay)}
            onToggleRead={() => toggleDay(currentDay)}
          />
        ) : (
          <div className="text-center py-20">
            <BookOpen className="h-12 w-12 mx-auto mb-4 opacity-50" />
            <h2 className="text-2xl mb-2">Em breve</h2>
            <p>O devocional do dia {currentDay} ainda não está disponível.</p>
          </div>
        )}
      </main>

      {/* FOOTER */}
      <footer className="border-t py-8 text-center">
        <p className="text-sm uppercase tracking-wide opacity-70">
          190 Dias com Deus
        </p>
        <p className="text-xs opacity-50 mt-1">
          Uma jornada de fé e transformação ✝️
        </p>
      </footer>
    </div>
  );
};

export default Index;
