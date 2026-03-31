import React, { useState, useEffect } from 'react';
import { Sparkles, Gift, PartyPopper, ChevronRight, RefreshCcw, ArrowRight } from 'lucide-react';

// Custom CSS for animations
const CustomStyles = () => (
  <style>
    {`
      @keyframes float {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-10px); }
      }
      @keyframes flicker {
        0%, 100% { opacity: 1; transform: scale(1); }
        50% { opacity: 0.8; transform: scale(1.1); }
      }
      @keyframes shake {
        0%, 100% { transform: rotate(0deg); }
        25% { transform: rotate(-5deg); }
        50% { transform: rotate(5deg); }
        75% { transform: rotate(-5deg); }
      }
      @keyframes pop-in {
        0% { transform: scale(0.8); opacity: 0; }
        100% { transform: scale(1); opacity: 1; }
      }
      @keyframes fall {
        0% { transform: translateY(-10vh) rotate(0deg); opacity: 1; }
        100% { transform: translateY(110vh) rotate(720deg); opacity: 0; }
      }
      .animate-float { animation: float 3s ease-in-out infinite; }
      .animate-flicker { animation: flicker 0.5s ease-in-out infinite alternate; transform-origin: bottom center; }
      .animate-shake { animation: shake 0.5s ease-in-out infinite; }
      .animate-pop-in { animation: pop-in 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards; }
      
      .confetti-piece {
        position: absolute;
        top: -20px;
        width: 10px;
        height: 20px;
        border-radius: 2px;
        animation: fall linear forwards;
      }
    `}
  </style>
);

// Confetti Component
const Confetti = () => {
  const colors = ['#f43f5e', '#ec4899', '#d946ef', '#8b5cf6', '#3b82f6', '#06b6d4', '#10b981', '#f59e0b'];
  const pieces = Array.from({ length: 75 }).map((_, i) => ({
    id: i,
    left: `${Math.random() * 100}%`,
    backgroundColor: colors[Math.floor(Math.random() * colors.length)],
    animationDuration: `${Math.random() * 3 + 2}s`,
    animationDelay: `${Math.random() * 2}s`,
    transform: `rotate(${Math.random() * 360}deg)`
  }));

  return (
    <div className="fixed inset-0 pointer-events-none overflow-hidden z-50">
      {pieces.map((p) => (
        <div
          key={p.id}
          className="confetti-piece"
          style={{
            left: p.left,
            backgroundColor: p.backgroundColor,
            animationDuration: p.animationDuration,
            animationDelay: p.animationDelay,
          }}
        />
      ))}
    </div>
  );
};

export default function App() {
  const [stage, setStage] = useState('intro'); // intro, cake, gift, party
  const [name, setName] = useState('');
  const [candlesLit, setCandlesLit] = useState(true);
  const [isOpeningGift, setIsOpeningGift] = useState(false);

  const handleStart = (e) => {
    e.preventDefault();
    if (name.trim() === '') setName('Friend');
    setStage('cake');
  };

  const blowCandles = () => {
    setCandlesLit(false);
    setTimeout(() => {
      setStage('gift');
    }, 2000);
  };

  const openGift = () => {
    setIsOpeningGift(true);
    setTimeout(() => {
      setStage('party');
    }, 1000);
  };

  const resetApp = () => {
    setStage('intro');
    setName('');
    setCandlesLit(true);
    setIsOpeningGift(false);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-indigo-900 via-purple-800 to-fuchsia-900 flex flex-col items-center justify-center p-4 font-sans text-slate-800 overflow-hidden relative">
      <CustomStyles />
      
      {/* Dynamic Background Elements */}
      <div className="absolute top-10 left-10 text-white/10 animate-float" style={{ animationDelay: '0s' }}><Sparkles size={48} /></div>
      <div className="absolute bottom-20 right-10 text-white/10 animate-float" style={{ animationDelay: '1s' }}><PartyPopper size={64} /></div>
      <div className="absolute top-1/3 right-1/4 text-white/10 animate-float" style={{ animationDelay: '2s' }}><Gift size={32} /></div>
      
      <div className="w-full max-w-md bg-white/95 backdrop-blur-sm rounded-3xl shadow-2xl p-8 text-center relative z-10 animate-pop-in">
        
        {/* STAGE 1: INTRO */}
        {stage === 'intro' && (
          <div className="space-y-6">
            <div className="flex justify-center mb-4">
              <div className="bg-pink-100 p-4 rounded-full text-pink-500">
                <PartyPopper size={48} />
              </div>
            </div>
            <h1 className="text-3xl md:text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-purple-600 to-pink-500">
              Let's Celebrate!
            </h1>
            <p className="text-gray-500 font-medium">Whose birthday is it today?</p>
            
            <form onSubmit={handleStart} className="space-y-4 mt-8">
              <input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
                placeholder="Enter name here..."
                className="w-full px-5 py-4 rounded-xl border-2 border-purple-100 focus:border-purple-400 focus:ring-4 focus:ring-purple-100 outline-none transition-all text-lg text-center font-semibold text-purple-900"
                autoFocus
              />
              <button
                type="submit"
                className="w-full bg-gradient-to-r from-purple-500 to-pink-500 hover:from-purple-600 hover:to-pink-600 text-white font-bold py-4 rounded-xl transition-transform transform hover:scale-[1.02] active:scale-95 flex items-center justify-center gap-2 shadow-lg shadow-purple-500/30"
              >
                Let's Go <ArrowRight size={20} />
              </button>
            </form>
          </div>
        )}

        {/* STAGE 2: CAKE */}
        {stage === 'cake' && (
          <div className="space-y-8 animate-pop-in">
            <h2 className="text-3xl font-bold text-purple-800">
              Make a Wish, <span className="text-pink-500">{name || 'Friend'}</span>!
            </h2>
            
            <div className="flex justify-center py-8">
              <svg width="200" height="200" viewBox="0 0 200 200" className="animate-float">
                {/* Plate */}
                <ellipse cx="100" cy="170" rx="80" ry="15" fill="#E2E8F0" />
                <ellipse cx="100" cy="168" rx="75" ry="12" fill="#F8FAFC" />
                
                {/* Cake Base Layer */}
                <rect x="50" y="110" width="100" height="50" rx="5" fill="#FDE68A" />
                <path d="M50 115 Q 60 125, 75 115 T 100 115 T 125 115 T 150 115 L 150 110 L 50 110 Z" fill="#FBBF24" />
                
                {/* Cake Middle Layer */}
                <rect x="60" y="70" width="80" height="40" rx="5" fill="#FBCFE8" />
                <path d="M60 75 Q 70 85, 80 75 T 100 75 T 120 75 T 140 75 L 140 70 L 60 70 Z" fill="#F472B6" />
                
                {/* Cake Top Layer */}
                <rect x="70" y="40" width="60" height="30" rx="5" fill="#C4B5FD" />
                <path d="M70 45 Q 80 55, 85 45 T 100 45 T 115 45 T 130 45 L 130 40 L 70 40 Z" fill="#A78BFA" />

                {/* Candles */}
                <rect x="85" y="15" width="6" height="25" fill="#CBD5E1" />
                <rect x="109" y="15" width="6" height="25" fill="#CBD5E1" />
                
                {/* Stripes on candles */}
                <line x1="85" y1="20" x2="91" y2="25" stroke="#EF4444" strokeWidth="2" />
                <line x1="85" y1="30" x2="91" y2="35" stroke="#EF4444" strokeWidth="2" />
                <line x1="109" y1="20" x2="115" y2="25" stroke="#EF4444" strokeWidth="2" />
                <line x1="109" y1="30" x2="115" y2="35" stroke="#EF4444" strokeWidth="2" />

                {/* Flames */}
                {candlesLit && (
                  <g className="animate-flicker">
                    <path d="M88 12 Q 85 5, 88 0 Q 91 5, 88 12 Z" fill="#F59E0B" />
                    <path d="M88 9 Q 86.5 4, 88 2 Q 89.5 4, 88 9 Z" fill="#FEF08A" />
                    
                    <path d="M112 12 Q 109 5, 112 0 Q 115 5, 112 12 Z" fill="#F59E0B" />
                    <path d="M112 9 Q 110.5 4, 112 2 Q 113.5 4, 112 9 Z" fill="#FEF08A" />
                  </g>
                )}
              </svg>
            </div>

            <button
              onClick={blowCandles}
              disabled={!candlesLit}
              className={`w-full py-4 rounded-xl font-bold transition-all transform ${
                candlesLit 
                  ? 'bg-gradient-to-r from-orange-400 to-red-500 text-white hover:scale-[1.02] active:scale-95 shadow-lg shadow-orange-500/30 cursor-pointer' 
                  : 'bg-gray-100 text-gray-400 cursor-not-allowed'
              }`}
            >
              {candlesLit ? "Blow out the candles!" : "Yay! Setting up your gift..."}
            </button>
          </div>
        )}

        {/* STAGE 3: GIFT */}
        {stage === 'gift' && (
          <div className="space-y-8 animate-pop-in">
            <h2 className="text-3xl font-bold text-purple-800">
              Here is a present for you!
            </h2>
            
            <div className="flex justify-center py-8">
              <button 
                onClick={openGift} 
                disabled={isOpeningGift}
                className="focus:outline-none focus:ring-4 focus:ring-purple-300 rounded-3xl p-4 transition-transform hover:scale-105 active:scale-95"
              >
                <svg width="180" height="180" viewBox="0 0 200 200" className={isOpeningGift ? '' : 'animate-shake'}>
                  {/* Gift Box Base */}
                  <rect x="40" y="80" width="120" height="100" fill="#3B82F6" rx="4" />
                  
                  {/* Vertical Ribbon */}
                  <rect x="90" y="80" width="20" height="100" fill="#FBBF24" />
                  
                  {/* Gift Box Lid */}
                  <rect x="30" y="60" width="140" height="30" fill="#2563EB" rx="4" />
                  
                  {/* Vertical Ribbon on Lid */}
                  <rect x="90" y="60" width="20" height="30" fill="#F59E0B" />
                  
                  {/* Bow Left Loop */}
                  <path d="M 100 60 C 70 30, 40 50, 95 60 Z" fill="#FBBF24" />
                  {/* Bow Right Loop */}
                  <path d="M 100 60 C 130 30, 160 50, 105 60 Z" fill="#FBBF24" />
                  {/* Bow Center */}
                  <circle cx="100" cy="60" r="8" fill="#F59E0B" />
                </svg>
              </button>
            </div>

            <p className="text-gray-500 font-medium animate-pulse">
              {isOpeningGift ? "Opening..." : "Tap the box to open it!"}
            </p>
          </div>
        )}

        {/* STAGE 4: PARTY/MESSAGE */}
        {stage === 'party' && (
          <div className="space-y-6 animate-pop-in relative">
            <Confetti />
            
            <div className="flex justify-center mb-2">
              <div className="bg-yellow-100 p-4 rounded-full text-yellow-500 mb-2">
                <Sparkles size={48} />
              </div>
            </div>
            
            <h1 className="text-4xl md:text-5xl font-black text-transparent bg-clip-text bg-gradient-to-r from-pink-500 via-purple-500 to-indigo-500 pb-2">
              HAPPY BIRTHDAY!
            </h1>
            
            <h2 className="text-2xl font-bold text-gray-800">
              {name || 'Friend'}
            </h2>
            
            <div className="bg-gradient-to-br from-purple-50 to-pink-50 p-6 rounded-2xl border border-pink-100 shadow-inner my-6">
              <p className="text-lg text-purple-900 font-medium leading-relaxed">
                Wishing you a day filled with joy, laughter, and unforgettable moments. May this coming year bring you success, peace, and all the wonderful things you deserve!
              </p>
            </div>
            
            <button
              onClick={resetApp}
              className="mt-4 mx-auto bg-gray-100 hover:bg-gray-200 text-gray-600 font-bold py-3 px-6 rounded-xl transition-colors flex items-center justify-center gap-2"
            >
              <RefreshCcw size={18} /> Play Again
            </button>
          </div>
        )}
      </div>
    </div>
  );
}

