import React, { useState, useEffect } from 'react';
import { 
  Printer, Activity, Utensils, Droplets, User, Dumbbell, Flame, 
  TrendingDown, Clipboard, ShoppingBag, Pill, Scale, Brain, 
  Moon, Footprints, Calendar, Clock, ChevronRight, Zap, 
  CheckCircle, CheckSquare, AlertTriangle, Lock, Unlock, List, Leaf, Info,
  Copy, RefreshCw, ChevronDown, Plus, Minus, Star, Coffee, Sun, Moon as MoonIcon, Sunset, HelpCircle, Shield, Truck, Repeat, MessageSquare, Eye, EyeOff, FileText, PenTool, Mic
} from 'lucide-react';

/**
 * CENTERVILLE METABOLIC TRANSFORMATION SYSTEM v17.0 (Command Center Edition)
 * Features: Interactive Consult Notes, Expanded Battle Script, CRM Integration
 */

// --- UTC OFFICIAL FOOD DATABASE (CORRECTED FACTORS) ---
// Factor = Grams of Macro per 1 Unit
const FOOD_DB = {
  proteins: [
    // Seafood
    { id: 'ahi_tuna', label: 'Ahi Tuna Steak (Raw)', factor: 7, unit: 'oz', icon: '🐟', recommended: true },
    { id: 'halibut', label: 'Halibut (Cooked)', factor: 6, unit: 'oz', icon: '🐟', recommended: true },
    { id: 'salmon', label: 'Salmon (Cooked)', factor: 7, unit: 'oz', icon: '🍣', recommended: true },
    { id: 'scallops', label: 'Scallops (Cooked)', factor: 6, unit: 'oz', icon: '🐚' },
    { id: 'shrimp', label: 'Shrimp (Cooked)', factor: 7, unit: 'oz', icon: '🍤' },
    { id: 'tilapia', label: 'Tilapia (Cooked)', factor: 7, unit: 'oz', icon: '🐟' },
    { id: 'tuna_can', label: 'Canned Tuna (Drained)', factor: 7, unit: 'oz', icon: '🥫' },
    { id: 'cod', label: 'Cod (Cooked)', factor: 6, unit: 'oz', icon: '🐟' },
    // Beef
    { id: 'beef_ground', label: 'Lean Grnd Beef (Cooked)', factor: 8, unit: 'oz', icon: '🥩', recommended: true },
    { id: 'steak_top_sirloin', label: 'Top Sirloin (Cooked)', factor: 8.5, unit: 'oz', icon: '🥩' },
    { id: 'steak_sirloin_side', label: 'Sirloin Tip (Cooked)', factor: 8, unit: 'oz', icon: '🥩' },
    // Poultry & Other
    { id: 'chicken', label: 'Chicken Breast (Cooked)', factor: 8.5, unit: 'oz', icon: '🍗', recommended: true },
    { id: 'chicken_thigh', label: 'Chicken Thigh (Cooked)', factor: 7, unit: 'oz', icon: '🍗' },
    { id: 'turkey_ground', label: 'Grnd Turkey (Cooked)', factor: 7.5, unit: 'oz', icon: '🦃' },
    { id: 'deli_meat', label: 'Deli Meat (Nitrate-Free)', factor: 5, unit: 'oz', icon: '🥪' },
    { id: 'pork_tender', label: 'Pork Tenderloin (Cooked)', factor: 7.5, unit: 'oz', icon: '🥓' },
    { id: 'venison', label: 'Venison (Cooked)', factor: 8.5, unit: 'oz', icon: '🦌' },
    { id: 'bison', label: 'Bison (Cooked)', factor: 8, unit: 'oz', icon: '🐃' },
    // Vegetarian/Dairy
    { id: 'egg_whites_lrg', label: 'Egg Whites (Large)', factor: 3.6, unit: 'egg', icon: '🥚', recommended: true },
    { id: 'egg_whites_cup', label: 'Egg Whites (Liquid)', factor: 26, unit: 'cup', icon: '🥛', recommended: true },
    { id: 'eggs', label: 'Whole Eggs', factor: 6, unit: 'egg', icon: '🍳' },
    { id: 'cottage', label: 'Cottage Cheese (2%)', factor: 28, unit: 'cup', icon: '🥣' },
    { id: 'yogurt', label: 'Greek Yogurt (0%)', factor: 22, unit: 'cup', icon: '🥣' },
    { id: 'whey', label: 'Whey Protein', factor: 24, unit: 'scoop', icon: '🥤' },
    { id: 'tempeh', label: 'Tempeh', factor: 5, unit: 'oz', icon: '🫘' },
    { id: 'tofu', label: 'Tofu (Firm)', factor: 2.5, unit: 'oz', icon: '🧊' },
    { id: 'beans', label: 'Beans/Lentils (Cooked)', factor: 14, unit: 'cup', icon: '🫘' },
  ],
  carbs: [
    // Grains (Updated Factors)
    { id: 'rice_jasmine', label: 'Jasmine Rice (Cooked)', factor: 45, unit: 'cup', icon: '🍚', recommended: true },
    { id: 'rice_brown', label: 'Brown Rice (Cooked)', factor: 45, unit: 'cup', icon: '🍚' },
    { id: 'quinoa', label: 'Quinoa (Cooked)', factor: 40, unit: 'cup', icon: '🌾' },
    { id: 'oats_old', label: 'Oats (Dry Measure)', factor: 54, unit: 'cup', icon: '🥣', recommended: true },
    { id: 'cream_rice', label: 'Cream of Rice (Dry)', factor: 36, unit: 'scoop', icon: '🥣' },
    // Starchy Veg
    { id: 'potato_white', label: 'White Potato (Cooked)', factor: 6, unit: 'oz', icon: '🥔', recommended: true },
    { id: 'potato_sweet', label: 'Sweet Potato (Cooked)', factor: 6, unit: 'oz', icon: '🍠' },
    { id: 'corn', label: 'Corn', factor: 30, unit: 'cup', icon: '🌽' },
    // Fruit
    { id: 'fruit_apple', label: 'Apple', factor: 4, unit: 'oz', icon: '🍎' },
    { id: 'fruit_banana', label: 'Banana', factor: 6, unit: 'oz', icon: '🍌' },
    { id: 'fruit_berry', label: 'Berries', factor: 3.5, unit: 'oz', icon: '🍓', recommended: true },
    { id: 'fruit_orange', label: 'Orange', factor: 3.5, unit: 'oz', icon: '🍊' },
    { id: 'fruit_pineapple', label: 'Pineapple', factor: 4, unit: 'oz', icon: '🍍' },
    { id: 'fruit_grapes', label: 'Grapes', factor: 5, unit: 'oz', icon: '🍇' },
    // Bread
    { id: 'bread_ez', label: 'Ezekiel English Muffin', factor: 26, unit: 'muffin', icon: '🍞' },
    { id: 'bread_wrap', label: 'Whole Wheat Wrap', factor: 22, unit: 'wrap', icon: '🌯' },
  ],
  fats: [
    // Nuts & Seeds
    { id: 'almonds', label: 'Almonds', factor: 14, unit: 'oz', icon: '🌰', recommended: true },
    { id: 'walnuts', label: 'Walnuts', factor: 18, unit: 'oz', icon: '🌰' },
    { id: 'cashews', label: 'Cashews', factor: 12, unit: 'oz', icon: '🌰' },
    { id: 'pumpkin_seeds', label: 'Pumpkin Seeds', factor: 14, unit: 'oz', icon: '🌱' },
    // Oils & Butters
    { id: 'avocado', label: 'Avocado', factor: 4, unit: 'oz', icon: '🥑', recommended: true },
    { id: 'oil_olive', label: 'Olive Oil', factor: 14, unit: 'tbsp', icon: '🫒' },
    { id: 'oil_coconut', label: 'Coconut Oil', factor: 14, unit: 'tbsp', icon: '🥥' },
    { id: 'butter', label: 'Kerry Gold Butter', factor: 12, unit: 'tbsp', icon: '🧈' },
    // Spreads
    { id: 'pb', label: 'Natural Nut Butter', factor: 8, unit: 'tbsp', icon: '🥜' },
    { id: 'guacamole', label: 'Guacamole', factor: 3, unit: 'tbsp', icon: '🥑' },
    { id: 'cheese', label: 'Cheese', factor: 9, unit: 'oz', icon: '🧀' },
    { id: 'yolks', label: 'Egg Yolks', factor: 5, unit: 'yolk', icon: '🍳' },
  ]
};

const UNLIMITED_VEG = [
  "Asparagus", "Broccoli", "Brussels Sprouts", "Cabbage", "Cauliflower", "Celery", 
  "Cucumber", "Eggplant", "Green Beans", "Greens (Kale, Collard, Mustard)", 
  "Mushrooms", "Peppers", "Salad Greens (Spinach, Romaine)", "Zucchini", "Sauerkraut/Kimchi"
];

const FREE_CONDIMENTS = [
  "Vinegar (Apple Cider, Balsamic, Red/White Wine)", "Mustard (Dijon/Yellow)", 
  "Hot Sauce (Franks)", "Salsa (<2g sugar)", "Herbs & Spices (Fresh/Dried)", 
  "Garlic", "Lemon/Lime Juice", "Braggs Liquid Aminos", "Coconut Aminos", 
  "Coffee/Tea (Black)", "Stevia"
];

// --- LOGIC HELPERS ---
const CPP_TABLE = {
  female: { ecto: [{max:150,val:11},{max:9999,val:9}], meso: [{max:150,val:10},{max:9999,val:8.5}], endo: [{max:150,val:9},{max:9999,val:7.5}] },
  male: { ecto: [{max:150,val:12},{max:9999,val:10}], meso: [{max:150,val:11},{max:9999,val:9}], endo: [{max:150,val:10},{max:9999,val:8}] }
};
const PROTEIN_TABLE_WOMEN = [{ max: 150, val: 0.8 }, { max: 175, val: 0.75 }, { max: 200, val: 0.7 }, { max: 9999, val: 0.6 }];
const PROTEIN_TABLE_MEN = [{ max: 200, val: 1.0 }, { max: 225, val: 0.95 }, { max: 250, val: 0.9 }, { max: 9999, val: 0.8 }];

const getRecs = (w, g, b, p) => {
  const pTable = g === 'female' ? PROTEIN_TABLE_WOMEN : PROTEIN_TABLE_MEN;
  const pMult = pTable.find(r => w <= r.max)?.val || 0.6;
  const cTable = CPP_TABLE[g][b];
  const cppBase = cTable.find(r => w <= r.max)?.val || 10;
  // DEFICIT LOGIC
  let deficit = 0;
  if (p === 'm1-deplete') deficit = 0.25; 
  else if (p === 'm1-reset') deficit = 0.15; 
  else if (p === 'm2-lifestyle') deficit = 0.05; 
  
  const cpp = cppBase * (1 - deficit);
  return { pMult, cpp };
};

// --- CONSULTATION SCRIPT DATA (MASTER SOP) ---
const CONSULT_SCRIPT = [
  {
    step: 1,
    title: "THE FRAME (AUTHORITY)",
    content: [
      { type: 'say', label: "OPENER", text: "Hey [Name]! Thanks for coming. I want to set the stage so you know exactly what to expect." },
      { type: 'say', label: "THE RULES", text: "Fill this out honestly. Don't sugarcoat it. The more honest you are, the better I can help." },
      { type: 'ask', label: "TIME CHECK", text: "Are you in a rush? We're going to be thorough, takes about 45 mins." },
      { type: 'action', text: "WALK AWAY. Give them 10 mins alone. Establish control." }
    ]
  },
  {
    step: 2,
    title: "RAPPORT & DATA",
    content: [
      { type: 'ask', label: "F.O.R.D.", text: "Ask about: Family, Occupation, Recreation, Dreams. (Lower anxiety)." },
      { type: 'ask', label: "WAIST", text: "Grab a waist measurement. 'Think Supermodel Pose'. (Establishes medical frame)." }
    ]
  },
  {
    step: 3,
    title: "DISCOVERY (THE PAIN)",
    content: [
      { type: 'ask', label: "HAPPINESS GAP", text: "You rated your health a 6. Why that number? Why isn't it a zero?" },
      { type: 'ask', label: "THE DIG", text: "How is this affecting you RIGHT NOW? (Energy? Kids? Focus?)" },
      { type: 'ask', label: "MAGIC WAND", text: "If I could fix this today, what would that be worth to you?" },
      { type: 'say', label: "ANCHOR", text: "Priceless? Okay. I ask because some people act like their health is worth $10/mo." }
    ]
  },
  {
    step: 4,
    title: "THE GAP (LOGIC)",
    content: [
      { type: 'say', label: "SHOW DATA", text: "Here is where you are [Weight]. Here is healthy [Goal]." },
      { type: 'ask', label: "THE REALITY", text: "When you see those numbers in black and white... what goes through your mind?" },
      { type: 'ask', label: "AGREEMENT", text: "So the mission is clear. We need to lose X lbs. Agreed?" }
    ]
  },
  {
    step: 5,
    title: "THE QUALIFIER",
    content: [
      { type: 'say', label: "FLIP SCRIPT", text: "I can help, but I only work with people who are ready. I need 3 Yeses:" },
      { type: 'ask', label: "Q1", text: "1. Willing to get out of your Comfort Zone?" },
      { type: 'ask', label: "Q2", text: "2. Willing to be Coachable?" },
      { type: 'ask', label: "Q3", text: "3. Willing to take Personal Responsibility?" }
    ]
  },
  {
    step: 6,
    title: "THE CLOSE (ASSUMPTIVE)",
    content: [
      { type: 'say', label: "CONFIDENCE", text: "Awesome. I'm super confident you're going to crush this." },
      { type: 'say', label: "THE ASK", text: "Last thing: Just confirming we are good to continue your membership after the challenge so you don't lose your spot. It's just $159/mo." },
      { type: 'action', text: "SHUT UP. Count to 10. First one to speak loses." }
    ]
  },
  {
    step: 7,
    title: "OBJECTION BATTLE CARDS",
    content: [
      { type: 'if', label: "IF: SILENCE", text: "Fair enough. Look, sign up today to keep momentum. I'll give you a 30-day 'Fair Enough' guarantee. If you don't love it, we part friends." },
      { type: 'if', label: "IF: SPOUSE", text: "If you went home today and said 'I'm doing this for my health', would they support you?" },
      { type: 'if', label: "IF: THINK", text: "I get it. But results don't happen in the 'Think About It' zone. Let's start." }
    ]
  }
];

// --- COMPONENTS ---

const FoodButton = ({ item, selected, onClick, locked }) => (
  <button
    onClick={() => !locked && onClick(item.id)}
    className={`
      relative p-2 border transition-all duration-150 flex flex-col items-center justify-center gap-1 group rounded-lg
      ${locked ? 'opacity-40 grayscale cursor-not-allowed border-slate-200 bg-slate-100' : ''}
      ${selected 
        ? 'border-red-600 bg-red-50 z-10 ring-1 ring-red-600 shadow-sm' 
        : 'border-slate-200 bg-white hover:border-red-400 text-slate-600 hover:shadow-md'}
    `}
  >
    <span className="text-xl group-hover:scale-110 transition-transform duration-200">{item.icon}</span>
    <span className={`text-[9px] font-bold uppercase text-center leading-tight tracking-tight ${selected ? 'text-red-600' : 'text-slate-500'}`}>
      {item.label}
    </span>
    {item.recommended && !locked && (
      <div className="absolute top-1 right-1">
        <Star className={`w-2 h-2 ${selected ? 'text-red-600' : 'text-slate-300'}`} fill="currentColor" />
      </div>
    )}
    {locked && <Lock className="absolute top-2 right-2 w-3 h-3 text-slate-400" />}
  </button>
);

const MacroDisplay = ({ label, amount, unit, foodName, color, subtext, isPhase1Carb }) => (
  <div className={`flex flex-col items-center justify-center p-4 border border-slate-200 bg-white relative overflow-hidden h-full shadow-sm rounded-xl`}>
    <div className={`absolute top-0 left-0 w-1 h-full bg-${color}-600`}></div>
    <div className={`text-[9px] font-black uppercase tracking-widest text-${color}-600 mb-1 z-10 w-full text-left pl-2`}>{label}</div>
    <div className="text-3xl font-black text-slate-900 z-10 flex items-baseline w-full pl-2">
      {isPhase1Carb ? "<30" : amount} <span className="text-xs font-bold text-slate-400 ml-1">{unit}</span>
    </div>
    <div className="text-xs text-slate-600 font-medium mt-1 z-10 truncate max-w-full px-2 w-full text-left pl-2">
      {foodName}
    </div>
    {subtext && <div className="text-[9px] text-slate-400 font-bold uppercase mt-2 pt-2 border-t border-slate-100 w-full text-left pl-2">{subtext}</div>}
  </div>
);

const ConsultAccordion = ({ isOpen, toggle, notes, setNotes }) => {
  if (!isOpen) return (
    <button onClick={toggle} className="w-full bg-slate-800 text-white p-4 rounded-xl flex items-center justify-between shadow-lg hover:bg-slate-700 transition-all mb-6 border-l-4 border-emerald-500 group">
      <div className="flex items-center gap-3">
        <div className="bg-emerald-500/20 p-2 rounded-lg">
           <MessageSquare className="w-5 h-5 text-emerald-400" />
        </div>
        <div className="text-left">
           <div className="font-bold uppercase text-xs tracking-widest text-emerald-400">Consultation Mode</div>
           <div className="text-[10px] text-slate-400 font-medium">Click to Open Script & Notes</div>
        </div>
      </div>
      <ChevronDown className="w-4 h-4 text-slate-400 group-hover:text-white" />
    </button>
  );

  return (
    <div className="bg-slate-800 text-slate-200 rounded-xl overflow-hidden shadow-xl mb-6 border border-slate-700 flex flex-col h-[600px]">
      <button onClick={toggle} className="w-full bg-slate-900 p-4 flex items-center justify-between border-b border-slate-700 shrink-0">
         <div className="flex items-center gap-3">
           <div className="bg-emerald-500/20 p-2 rounded-lg">
              <Clipboard className="w-5 h-5 text-emerald-400" />
           </div>
           <div className="text-left">
              <div className="font-bold uppercase text-xs tracking-widest text-emerald-400">Tactical Script</div>
              <div className="text-[10px] text-slate-400 font-medium">Follow Steps 1-7 Precisely</div>
           </div>
         </div>
        <ChevronDown className="w-4 h-4 rotate-180 text-slate-400" />
      </button>
      
      <div className="flex-1 overflow-y-auto custom-scrollbar p-4 space-y-4">
        {CONSULT_SCRIPT.map((item) => (
          <div key={item.step} className="bg-slate-700/50 p-3 rounded-lg border-l-2 border-emerald-500">
            <h4 className="text-[10px] font-black text-emerald-400 uppercase mb-2 flex items-center justify-between">
               Step {item.step}: {item.title}
            </h4>
            {item.content.map((line, idx) => {
               const isAction = line.type === 'action';
               const isIf = line.type === 'if';
               const isAsk = line.type === 'ask';
               return (
                 <div key={idx} className={`text-xs mb-2 leading-relaxed ${isAction ? 'text-amber-400 font-bold italic bg-amber-400/10 p-2 rounded' : (isIf ? 'text-red-300 bg-red-500/10 p-2 rounded border border-red-500/20' : 'text-slate-300')}`}>
                   {line.label && <span className={`font-black uppercase mr-1 tracking-wider ${isAsk ? 'text-cyan-400' : (isIf ? 'text-red-400' : 'text-slate-500')}`}>{line.label}:</span>}
                   {line.text}
                 </div>
               );
            })}
          </div>
        ))}
      </div>

      <div className="p-4 bg-slate-900 border-t border-slate-700 shrink-0">
         <label className="text-[10px] font-bold uppercase text-slate-500 mb-1 flex items-center gap-1">
            <PenTool className="w-3 h-3" /> Consultation Notes (Saved to CRM)
         </label>
         <textarea 
            value={notes} 
            onChange={(e) => setNotes(e.target.value)}
            className="w-full h-24 bg-slate-800 border border-slate-700 rounded-lg p-2 text-xs text-white placeholder-slate-600 focus:outline-none focus:border-emerald-500 resize-none"
            placeholder="Type key insights, objections, or drivers here..."
         />
      </div>
    </div>
  );
};

// --- MAIN APP ---

export default function MTSApp() {
  const [mode, setMode] = useState('coach'); 
  const [showConsult, setShowConsult] = useState(false);
  const [shoppingDays, setShoppingDays] = useState(7);
  
  // INITIAL STATE & PERSISTENCE
  const initialState = {
    client: { 
      name: "Client Name", weight: 200, gender: "female", bodyType: "endo", bodyFat: 32, smm: 70, bmr: 1600 
    },
    consultNotes: "",
    program: { phase: "m1-deplete", meals: 4 },
    mealSelections: {
      1: { p: 'egg_whites_cup', c: 'oats_old', f: 'avocado' },
      2: { p: 'chicken', c: 'rice_jasmine', f: 'avocado' },
      3: { p: 'chicken', c: 'rice_jasmine', f: 'avocado' },
      4: { p: 'chicken', c: 'rice_jasmine', f: 'avocado' },
      5: { p: 'whey', c: 'rice_jasmine', f: 'almonds' },
      6: { p: 'cottage', c: 'fruit_berry', f: 'almonds' },
    }
  };

  const [data, setData] = useState(() => {
    const saved = localStorage.getItem('mts_data_v17');
    return saved ? JSON.parse(saved) : initialState;
  });

  const [activeMeal, setActiveMeal] = useState(1);

  // Auto-Save Effect
  useEffect(() => {
    localStorage.setItem('mts_data_v17', JSON.stringify(data));
  }, [data]);

  // Convenience Setters
  const setClient = (newClient) => setData(d => ({ ...d, client: newClient }));
  const setProgram = (newProgram) => setData(d => ({ ...d, program: newProgram }));
  const setMealSelections = (newSelections) => setData(d => ({ ...d, mealSelections: newSelections }));
  const setConsultNotes = (val) => setData(d => ({ ...d, consultNotes: val }));

  // Helper function to update specific meal selections safely
  const updateMealSelection = (mealId, type, foodId) => {
    setData(prev => ({
      ...prev,
      mealSelections: {
        ...prev.mealSelections,
        [mealId]: {
          ...prev.mealSelections[mealId],
          [type]: foodId
        }
      }
    }));
  };

  const { client, program, mealSelections, consultNotes } = data;

  // Constants
  const mealNames = { 1: "Breakfast", 2: "Lunch", 3: "Snack", 4: "Dinner", 5: "Post-Workout", 6: "Snack 2" };

  // Logic
  const { pMult, cpp } = getRecs(client.weight, client.gender, client.bodyType, program.phase);
  const dailyP = client.weight * pMult;
  const dailyCals = client.weight * cpp;
  const carbFactor = program.phase === 'm1-deplete' ? 0 : (program.phase === 'm1-reset' ? 0.35 : 0.8);
  const dailyC = client.weight * carbFactor;
  // Safety Floor for Fats (Ensure at least 30g)
  let rawDailyF = (dailyCals - (dailyP * 4) - (dailyC * 4)) / 9;
  const dailyF = Math.max(30, rawDailyF);
  const dailyWater = client.weight / 2;
  
  const targetP = dailyP / program.meals;
  const targetC = dailyC / program.meals;
  const targetF = dailyF / program.meals;

  const copyToAll = () => {
    const current = mealSelections[activeMeal];
    const newSelections = {};
    for (let i = 1; i <= 6; i++) newSelections[i] = { ...current }; // Init all 6 to safe
    setMealSelections(newSelections);
  };

  const getPortion = (type, id, target) => {
    const db = type === 'p' ? FOOD_DB.proteins : (type === 'c' ? FOOD_DB.carbs : FOOD_DB.fats);
    const item = db.find(x => x.id === id);
    if (!item || target === 0) return { val: 0, unit: '-', label: 'None', item: {label: 'None'} }; 
    // Rounding logic for clean numbers
    let val = (target / item.factor);
    if(item.unit === 'oz') val = Math.round(val * 2) / 2; // Nearest 0.5
    else val = Math.round(val * 10) / 10;
    
    return { val: val.toFixed(1), unit: item.unit, label: item.label, item };
  };

  const currentSel = mealSelections[activeMeal];
  const pData = getPortion('p', currentSel.p, targetP);
  const cData = getPortion('c', currentSel.c, targetC);
  const fData = getPortion('f', currentSel.f, targetF);

  // Smart Grocery List Generator
  const generateGroceryList = () => {
    const list = {};
    for (let i = 1; i <= program.meals; i++) {
      const s = mealSelections[i];
      const p = getPortion('p', s.p, targetP);
      if (p.val > 0) {
         const key = `${p.item.label}`;
         list[key] = { amount: (list[key]?.amount || 0) + parseFloat(p.val), unit: p.item.unit };
      }
      if (program.phase !== 'm1-deplete') {
        const c = getPortion('c', s.c, targetC);
        if (c.val > 0) {
           const key = `${c.item.label}`;
           list[key] = { amount: (list[key]?.amount || 0) + parseFloat(c.val), unit: c.item.unit };
        }
      }
      const f = getPortion('f', s.f, targetF);
      if (f.val > 0) {
         const key = `${f.item.label}`;
         list[key] = { amount: (list[key]?.amount || 0) + parseFloat(f.val), unit: f.item.unit };
      }
    }
    
    // Format for human
    return Object.keys(list).sort().reduce((acc, key) => {
        const item = list[key];
        const total = item.amount * shoppingDays; // Use State
        let display = `${total.toFixed(1)} ${item.unit}`;
        
        // Smart Conversions
        if (item.unit === 'oz' && total > 16) {
           display = `${total.toFixed(0)} oz (~${(total/16).toFixed(1)} lbs)`;
        } else if (item.unit === 'cup' && total > 4) {
           if (key.includes('Egg Whites')) {
              display = `${total.toFixed(1)} cups (~${(total/4).toFixed(0)} Cartons)`;
           } else {
              display = `${total.toFixed(1)} cups`;
           }
        } else if (item.unit === 'scoop' && total > 15) {
           display = `${total.toFixed(0)} scoops (~1 Tub)`;
        } else if (item.unit === 'egg' && total > 12) {
           display = `${total.toFixed(0)} eggs (~${(total/12).toFixed(0)} Dozen)`;
        }

        acc[key] = display;
        return acc;
    }, {});
  };

  const groceryList = generateGroceryList();

  // Reset Data Function
  const resetData = () => {
    if(window.confirm("Reset all client data?")) {
        setData(initialState);
    }
  }

  // Copy Summary to Clipboard
  const copySummary = () => {
    const summary = `Client: ${client.name} | Phase: ${program.phase} | P: ${dailyP.toFixed(0)}g | C: ${program.phase==='m1-deplete'?'<30':dailyC.toFixed(0)}g | F: ${dailyF.toFixed(0)}g | Cals: ${dailyCals.toFixed(0)}\nMeals: ${program.meals} | Type: ${client.bodyType} \n\nCONSULT NOTES:\n${consultNotes || 'No notes taken.'}\n\nGenerated via Centerville MTS`;
    navigator.clipboard.writeText(summary);
    alert("Plan Summary & Consult Notes copied to clipboard for CRM!");
  };

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 font-sans selection:bg-red-600 selection:text-white print:bg-white print:text-black">
      
      {/* --- HEADER (COACH vs CLIENT TOGGLE) --- */}
      <div className="max-w-7xl mx-auto p-4 md:p-6 print:hidden">
        <header className="flex justify-between items-center mb-8 border-b border-slate-200 pb-6 bg-white p-4 rounded-xl shadow-sm">
          <div className="flex items-center space-x-4">
            <div className="w-12 h-12 bg-red-600 flex items-center justify-center font-black text-xl text-white tracking-tighter rounded-lg transform -skew-x-12 shadow-lg shadow-red-200">
              C
            </div>
            <div>
              <h1 className="text-xl font-black text-slate-900 tracking-tight uppercase">Centerville MTS <span className="text-red-600">v17.0</span></h1>
              <p className="text-[10px] font-bold text-slate-400 tracking-widest uppercase">Performance Nutrition Protocol</p>
            </div>
          </div>
          <div className="flex space-x-4 items-center">
             <div className="flex bg-slate-100 p-1 border border-slate-200 rounded-lg">
               <button onClick={()=>setMode('coach')} className={`px-4 py-2 text-xs font-bold uppercase rounded-md transition-all ${mode==='coach' ? 'bg-white text-red-600 shadow-sm' : 'text-slate-400 hover:text-slate-600'}`}>Coach</button>
               <button onClick={()=>setMode('client')} className={`px-4 py-2 text-xs font-bold uppercase rounded-md transition-all ${mode==='client' ? 'bg-white text-green-600 shadow-sm' : 'text-slate-400 hover:text-slate-600'}`}>Client</button>
             </div>
             <button onClick={resetData} className="p-2 text-slate-400 hover:text-red-600" title="Reset Data">
                <RefreshCw className="w-5 h-5" />
             </button>
             <button onClick={copySummary} className="bg-slate-800 text-white px-4 py-2 rounded-lg font-bold flex items-center gap-2 hover:bg-slate-700 transition-colors text-xs uppercase tracking-wider shadow-md">
               <FileText className="w-4 h-4" /> Copy for CRM
             </button>
             <button onClick={() => window.print()} className="bg-red-600 text-white px-6 py-2 rounded-lg font-bold flex items-center gap-2 hover:bg-red-700 transition-colors text-xs uppercase tracking-wider shadow-lg shadow-red-200">
               <Printer className="w-4 h-4" /> Print Plan
             </button>
          </div>
        </header>

        <div className="grid grid-cols-1 lg:grid-cols-12 gap-8">
          
          {/* --- LEFT: CONTROLS (Hidden in Client Mode) --- */}
          {mode === 'coach' && (
            <div className="lg:col-span-4 space-y-6">
              
              {/* CONSULTATION CHEAT SHEET (COLLAPSIBLE) */}
              <ConsultAccordion 
                 isOpen={showConsult} 
                 toggle={() => setShowConsult(!showConsult)} 
                 notes={consultNotes}
                 setNotes={setConsultNotes}
              />

              {/* Visual Gap */}
              <div className="bg-white border border-slate-200 rounded-2xl p-6 shadow-sm">
                 <div className="flex justify-between items-center mb-6">
                   <h3 className="font-bold text-slate-800 uppercase text-xs flex items-center"><Activity className="w-4 h-4 mr-2 text-red-600" /> InBody Metrics</h3>
                 </div>
                 
                 {/* Client Name Input */}
                 <div className="mb-4">
                    <label className="text-[9px] text-slate-400 uppercase font-bold mb-1 block">Client Name</label>
                    <input type="text" value={client.name} onChange={e=>setClient({...client, name: e.target.value})} className="w-full bg-slate-50 border border-slate-200 rounded-lg p-2 text-slate-900 text-sm font-bold focus:border-red-500 outline-none" />
                 </div>

                 <div className="grid grid-cols-2 gap-4 mb-4">
                   <div>
                     <label className="text-[9px] text-slate-400 uppercase font-bold mb-1 block">Gender</label>
                     <select value={client.gender} onChange={e=>setClient({...client, gender: e.target.value})} className="w-full bg-slate-50 border border-slate-200 rounded-lg p-2 text-slate-900 text-sm font-bold focus:border-red-500 outline-none">
                       <option value="female">Female</option>
                       <option value="male">Male</option>
                     </select>
                   </div>
                   <div>
                     <label className="text-[9px] text-slate-400 uppercase font-bold mb-1 block">Body Type</label>
                     <select value={client.bodyType} onChange={e=>setClient({...client, bodyType: e.target.value})} className="w-full bg-slate-50 border border-slate-200 rounded-lg p-2 text-slate-900 text-sm font-bold focus:border-red-500 outline-none">
                       <option value="endo">Endomorph</option>
                       <option value="meso">Mesomorph</option>
                       <option value="ecto">Ectomorph</option>
                     </select>
                   </div>
                 </div>
                 <div className="grid grid-cols-2 gap-4 mb-6">
                   <div>
                     <label className="text-[9px] text-slate-400 uppercase font-bold mb-1 block">Weight (lbs)</label>
                     <input type="number" value={client.weight} onChange={e=>setClient({...client, weight: +e.target.value})} className="w-full bg-slate-50 border border-slate-200 rounded-lg p-2 text-slate-900 text-sm font-bold focus:border-red-500 outline-none" />
                   </div>
                   <div>
                     <label className="text-[9px] text-slate-400 uppercase font-bold mb-1 block">Body Fat %</label>
                     <input type="number" value={client.bodyFat} onChange={e=>setClient({...client, bodyFat: +e.target.value})} className="w-full bg-slate-50 border border-slate-200 rounded-lg p-2 text-slate-900 text-sm font-bold focus:border-red-500 outline-none" />
                   </div>
                 </div>
                 
                 {/* MEALS PER DAY SELECTOR */}
                 <div className="mb-6">
                    <label className="text-[9px] text-slate-400 uppercase font-bold mb-1 block">Meals Per Day</label>
                    <div className="flex bg-slate-100 rounded-lg p-1">
                      {[3, 4, 5, 6].map(num => (
                        <button 
                          key={num}
                          onClick={() => setProgram({...program, meals: num})}
                          className={`flex-1 py-2 text-xs font-bold rounded transition-all ${program.meals === num ? 'bg-white shadow text-red-600' : 'text-slate-400 hover:text-slate-600'}`}
                        >
                          {num}
                        </button>
                      ))}
                    </div>
                 </div>

                 {/* SHOPPING DAYS SLIDER */}
                 <div className="mb-2">
                    <label className="text-[9px] text-slate-400 uppercase font-bold mb-1 block">Shopping List Duration</label>
                    <div className="flex items-center gap-4 bg-slate-50 p-3 rounded-lg border border-slate-100">
                        <input 
                          type="range" min="1" max="14" 
                          value={shoppingDays} 
                          onChange={(e) => setShoppingDays(parseInt(e.target.value))}
                          className="w-full accent-red-600 cursor-pointer"
                        />
                        <span className="font-black text-sm w-16 text-right text-slate-700">{shoppingDays} Days</span>
                    </div>
                 </div>

              </div>

              {/* Phase Selector */}
              <div className="grid grid-cols-1 gap-2">
                 {[
                   {id: 'm1-deplete', l: 'Phase 1: Adaptation', sub: 'Restore Sensitivity (25% Deficit)', c: 'red', info: 'We are depleting glycogen to force your liver to switch from burning sugar to burning fat. This is non-negotiable for 14 days.'},
                   {id: 'm1-reset', l: 'Phase 2: Acceleration', sub: 'Fuel Performance (15% Deficit)', c: 'blue', info: 'Reintroducing carbs strategically to restore leptin levels and fuel training performance.'},
                   {id: 'm2-lifestyle', l: 'Phase 3: Lifestyle', sub: 'Metabolic Flex (Maintenance)', c: 'green', info: 'Optimized for long-term sustainability and metabolic flexibility.'},
                 ].map(p => (
                   <div key={p.id} className="relative group">
                     <button 
                       onClick={() => setProgram({...program, phase: p.id})}
                       className={`
                         text-left w-full p-4 rounded-xl border-2 transition-all flex items-center justify-between shadow-sm
                         ${program.phase === p.id 
                           ? `bg-white border-${p.c}-600 ring-1 ring-${p.c}-600` 
                           : 'bg-white border-slate-200 hover:border-slate-300'}
                       `}
                     >
                       <div>
                         <div className={`text-xs font-black uppercase ${program.phase === p.id ? `text-${p.c}-600` : 'text-slate-500'}`}>{p.l}</div>
                         <div className="text-[10px] text-slate-400 font-bold uppercase mt-0.5">{p.sub}</div>
                       </div>
                       {program.phase === p.id && <CheckCircle className={`w-5 h-5 text-${p.c}-600`} />}
                     </button>
                   </div>
                 ))}
              </div>
            </div>
          )}

          {/* --- RIGHT: CLIENT EXPERIENCE --- */}
          <div className={`${mode === 'client' ? 'lg:col-span-12' : 'lg:col-span-8'}`}>
            
            {/* VISUAL NEXT MEAL (Client Mode Only) */}
            {mode === 'client' && (
              <div className="bg-white border-l-4 border-red-600 p-8 mb-8 relative overflow-hidden rounded-r-xl shadow-sm">
                 <div className="relative z-10 flex flex-col md:flex-row justify-between items-end gap-6">
                   <div>
                      <div className="text-red-600 font-bold uppercase tracking-widest text-[10px] mb-2">Current Objective</div>
                      <div className="text-4xl md:text-6xl font-black text-slate-900 uppercase mb-2 leading-none">{mealNames[activeMeal]}</div>
                      <div className="text-slate-400 text-sm font-medium flex items-center gap-2">
                        <Clock className="w-4 h-4" /> 
                        {activeMeal === 1 ? "8:00 AM" : activeMeal === 2 ? "12:00 PM" : activeMeal === 3 ? "3:00 PM" : "7:00 PM"}
                      </div>
                   </div>
                   <div className="flex gap-4">
                      <div className="bg-slate-50 border border-slate-200 p-4 min-w-[120px] rounded-lg">
                        <div className="text-xs text-slate-400 uppercase font-bold mb-1">Protein Target</div>
                        <div className="text-2xl font-black text-slate-900">{pData.val}<span className="text-sm text-slate-400 ml-1">{pData.unit}</span></div>
                        <div className="text-[10px] uppercase font-bold text-red-600 mt-1 truncate max-w-[100px]">{pData.label}</div>
                      </div>
                      <div className="bg-slate-50 border border-slate-200 p-4 min-w-[120px] rounded-lg">
                        <div className="text-xs text-slate-400 uppercase font-bold mb-1">Carb Target</div>
                        <div className="text-2xl font-black text-slate-900">{program.phase==='m1-deplete'?'<30':cData.val}<span className="text-sm text-slate-400 ml-1">{program.phase==='m1-deplete'?'g':cData.unit}</span></div>
                        <div className="text-[10px] uppercase font-bold text-red-600 mt-1 truncate max-w-[100px]">{program.phase==='m1-deplete'?'Trace Only':cData.label}</div>
                      </div>
                   </div>
                 </div>
              </div>
            )}

            <div className="border border-slate-200 bg-white rounded-2xl shadow-sm overflow-hidden">
              
              {/* Meal Tabs */}
              <div className="flex overflow-x-auto border-b border-slate-200">
                {Array.from({length: program.meals}).map((_, i) => {
                  const mId = i + 1;
                  return (
                    <button
                      key={mId}
                      onClick={() => setActiveMeal(mId)}
                      className={`
                        px-8 py-4 font-bold text-xs uppercase tracking-wider whitespace-nowrap transition-all flex items-center gap-2 border-r border-slate-100
                        ${activeMeal === mId 
                          ? 'bg-slate-50 text-red-600 border-b-2 border-b-red-600' 
                          : 'bg-white text-slate-400 hover:text-slate-600'}
                      `}
                    >
                      {mealNames[mId]}
                    </button>
                  );
                })}
                <button onClick={copyToAll} className="ml-auto px-6 py-4 text-[10px] font-bold uppercase text-slate-400 hover:text-red-600 flex items-center gap-2 bg-slate-50">
                  <Copy className="w-3 h-3" /> Duplicate
                </button>
              </div>

              {/* The Builder Stage */}
              <div className="p-6 lg:p-8">
                
                {/* Result Cards (Visible in Coach Mode) */}
                {mode === 'coach' && (
                  <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-10">
                     <MacroDisplay label="Protein" amount={pData.val} unit={pData.unit} foodName={pData.label} color="blue" subtext={`Target: ${targetP.toFixed(0)}g`} />
                     <MacroDisplay 
                       label="Carbs" 
                       amount={cData.val} 
                       unit={cData.unit} 
                       foodName={program.phase === 'm1-deplete' ? "Trace Veggies Only" : cData.label} 
                       color="green" 
                       subtext={program.phase === 'm1-deplete' ? "Phase 1 Protocol" : `Target: ${targetC.toFixed(0)}g`} 
                       isPhase1Carb={program.phase === 'm1-deplete'}
                     />
                     <MacroDisplay label="Fats" amount={fData.val} unit={fData.unit} foodName={fData.label} color="yellow" subtext={`Target: ${targetF.toFixed(0)}g`} />
                  </div>
                )}

                {/* Food Grids */}
                <div className="space-y-10">
                  
                  {/* Protein Grid */}
                  <div>
                    <h3 className="text-xs font-black uppercase text-slate-400 mb-4 flex items-center tracking-widest"><Dumbbell className="w-3 h-3 mr-2" /> Protein Source</h3>
                    <div className="grid grid-cols-4 sm:grid-cols-5 md:grid-cols-7 gap-2">
                      {FOOD_DB.proteins.filter(f=>f.recommended).map(f => <FoodButton key={f.id} item={f} selected={currentSel.p === f.id} onClick={(id) => updateMealSelection(activeMeal, 'p', id)} />)}
                      <div className="col-span-full h-px bg-slate-100 my-2"></div>
                      {FOOD_DB.proteins.filter(f=>!f.recommended).map(f => <FoodButton key={f.id} item={f} selected={currentSel.p === f.id} onClick={(id) => updateMealSelection(activeMeal, 'p', id)} />)}
                    </div>
                  </div>

                  {/* Carb Grid */}
                  <div>
                    <div className="flex justify-between items-center mb-4">
                       <h3 className="text-xs font-black uppercase text-slate-400 flex items-center tracking-widest"><Flame className="w-3 h-3 mr-2" /> Carb Source</h3>
                       {program.phase === 'm1-deplete' && <span className="text-[10px] font-bold bg-slate-100 border border-slate-200 text-slate-500 px-3 py-1 flex items-center rounded-full"><Shield className="w-3 h-3 mr-2" /> Phase 1 Protocol</span>}
                    </div>
                    <div className="grid grid-cols-4 sm:grid-cols-5 md:grid-cols-7 gap-2">
                      {FOOD_DB.carbs.filter(f=>f.recommended).map(f => <FoodButton key={f.id} item={f} selected={currentSel.c === f.id} onClick={(id) => updateMealSelection(activeMeal, 'c', id)} locked={program.phase === 'm1-deplete'} />)}
                      <div className="col-span-full h-px bg-slate-100 my-2"></div>
                      {FOOD_DB.carbs.filter(f=>!f.recommended).map(f => <FoodButton key={f.id} item={f} selected={currentSel.c === f.id} onClick={(id) => updateMealSelection(activeMeal, 'c', id)} locked={program.phase === 'm1-deplete'} />)}
                    </div>
                  </div>

                  {/* Fat Grid */}
                  <div>
                    <h3 className="text-xs font-black uppercase text-slate-400 mb-4 flex items-center tracking-widest"><Activity className="w-3 h-3 mr-2" /> Fat Source</h3>
                    <div className="grid grid-cols-4 sm:grid-cols-5 md:grid-cols-7 gap-2">
                      {FOOD_DB.fats.filter(f=>f.recommended).map(f => <FoodButton key={f.id} item={f} selected={currentSel.f === f.id} onClick={(id) => updateMealSelection(activeMeal, 'f', id)} />)}
                      <div className="col-span-full h-px bg-slate-100 my-2"></div>
                      {FOOD_DB.fats.filter(f=>!f.recommended).map(f => <FoodButton key={f.id} item={f} selected={currentSel.f === f.id} onClick={(id) => updateMealSelection(activeMeal, 'f', id)} />)}
                    </div>
                  </div>

                </div>

              </div>
              
              {/* RESTORED: THE CALCULATOR (Engineer View) */}
              <div className="border-t border-slate-200 p-6 lg:p-8 bg-slate-50">
                 <div className="flex items-center mb-6">
                    <List className="w-4 h-4 text-slate-400 mr-2" />
                    <h3 className="text-xs font-black uppercase text-slate-400 tracking-widest">Macro Engineering</h3>
                 </div>
                 
                 <div className="overflow-x-auto">
                    <table className="w-full text-xs text-left">
                       <thead className="text-slate-500 border-b border-slate-200">
                          <tr>
                             <th className="py-3 px-4 font-bold uppercase">Meal</th>
                             <th className="py-3 px-4 font-bold uppercase">Protein</th>
                             <th className="py-3 px-4 font-bold uppercase">Carbs</th>
                             <th className="py-3 px-4 font-bold uppercase">Fats</th>
                             <th className="py-3 px-4 font-bold uppercase text-right">Calories</th>
                          </tr>
                       </thead>
                       <tbody className="divide-y divide-slate-200">
                          {Array.from({length: program.meals}).map((_, i) => {
                             const mId = i+1;
                             const sel = mealSelections[mId];
                             const p = getPortion('p', sel.p, targetP);
                             const c = getPortion('c', sel.c, targetC);
                             const f = getPortion('f', sel.f, targetF);
                             const cals = (targetP * 4) + (targetC * 4) + (targetF * 9);
                             return (
                                <tr key={mId} className="text-slate-600 hover:bg-white transition-colors">
                                   <td className="py-3 px-4 font-bold text-slate-900">{mealNames[mId]}</td>
                                   <td className="py-3 px-4"><span className="text-blue-600 font-bold">{targetP.toFixed(0)}g</span> <span className="text-slate-400 ml-1">({p.item?.label || 'None'})</span></td>
                                   <td className="py-3 px-4"><span className="text-green-600 font-bold">{program.phase==='m1-deplete'?'<30':targetC.toFixed(0)}g</span> <span className="text-slate-400 ml-1">({program.phase !== 'm1-deplete' ? c.item?.label || 'None' : 'Trace Veggies'})</span></td>
                                   <td className="py-3 px-4"><span className="text-yellow-600 font-bold">{targetF.toFixed(0)}g</span> <span className="text-slate-400 ml-1">({f.item?.label || 'None'})</span></td>
                                   <td className="py-3 px-4 text-right font-mono text-slate-900 font-bold">{cals.toFixed(0)}</td>
                                </tr>
                             );
                          })}
                          <tr className="bg-white border-t border-slate-200 shadow-sm">
                             <td className="py-4 px-4 font-black text-slate-900 uppercase tracking-widest">Daily Totals</td>
                             <td className="py-4 px-4 font-black text-blue-600 text-lg">{dailyP.toFixed(0)}g</td>
                             <td className="py-4 px-4 font-black text-green-600 text-lg">{program.phase==='m1-deplete'?'<30':dailyC.toFixed(0)}g</td>
                             <td className="py-4 px-4 font-black text-yellow-600 text-lg">{dailyF.toFixed(0)}g</td>
                             <td className="py-4 px-4 font-black text-slate-900 text-lg text-right">{dailyCals.toFixed(0)}</td>
                          </tr>
                       </tbody>
                    </table>
                 </div>
              </div>

            </div>

          </div>
        </div>
      </div>

      {/* --- PRINT LAYOUT (The Prescription) --- */}
      <div className="hidden print:block p-8 max-w-[1000px] mx-auto text-black">
         {/* Print Header */}
         <div className="flex justify-between items-end border-b-4 border-red-600 pb-4 mb-8">
            <div>
               <h1 className="text-5xl font-black text-black uppercase leading-none tracking-tighter">CENTERVILLE<span className="text-red-600">.MTS</span></h1>
               <div className="text-sm font-bold uppercase tracking-widest mt-1 text-slate-500">Metabolic Nutrition Protocol • {program.phase.replace('m1-', '').replace('m2-', '').toUpperCase()}</div>
            </div>
            <div className="text-right">
               <div className="text-2xl font-black">{client.name}</div>
               <div className="text-sm font-mono text-slate-600">{new Date().toLocaleDateString()}</div>
            </div>
         </div>

         {/* Phase Rationale (Why) */}
         <div className="mb-8 p-4 bg-slate-50 border-l-4 border-red-600 rounded-r-lg break-inside-avoid">
            <h3 className="font-black uppercase text-sm text-red-600 mb-1">Phase Rationale: The "Why"</h3>
            <p className="text-sm text-slate-800 font-medium leading-relaxed">
               {program.phase === 'm1-deplete' && "We are temporarily restricting starchy carbohydrates to deplete glycogen stores. This biologically forces your liver to switch energy systems from burning sugar to burning stored body fat. This process restores insulin sensitivity and accelerates metabolic adaptation."}
               {program.phase === 'm1-reset' && "We are reintroducing controlled carbohydrates around training windows. This fuels high-intensity performance while keeping insulin levels managed during rest periods, ensuring fat loss continues while performance increases."}
               {program.phase === 'm2-lifestyle' && "We are focusing on metabolic flexibility. Higher caloric intake signals safety to your metabolism, preventing 'starvation mode' adaptation while maintaining lean muscle mass and long-term health."}
            </p>
         </div>

         {/* Weekly Schedule Strip */}
         <div className="mb-8 break-inside-avoid">
            <div className="flex border-2 border-black h-20">
               {['MON','TUE','WED','THU','FRI','SAT','SUN'].map((d, i) => (
                 <div key={d} className={`flex-1 border-r border-black last:border-r-0 flex flex-col items-center justify-center ${i === 6 && program.phase !== 'm1-deplete' ? 'bg-black text-white' : ''}`}>
                    <span className="text-[10px] font-bold opacity-60">{d}</span>
                    <span className="text-xs font-black uppercase">
                      {program.phase === 'm1-deplete' ? 'BURN' : (i===2 || i===5 ? 'REST' : (i===6 ? 'REFEED' : 'TRAIN'))}
                    </span>
                 </div>
               ))}
            </div>
         </div>

         {/* Meal Breakdown */}
         <div className="grid grid-cols-2 gap-8 mb-8 break-inside-avoid">
            {/* Left: Meal Schedule */}
            <div>
               <h3 className="font-black uppercase text-lg border-b-2 border-black mb-4">Daily Meal Schedule</h3>
               <div className="space-y-4">
                  {Array.from({length: program.meals}).map((_, i) => {
                     const mId = i+1;
                     const sel = mealSelections[mId];
                     const p = getPortion('p', sel.p, targetP);
                     const c = getPortion('c', sel.c, targetC);
                     const f = getPortion('f', sel.f, targetF);
                     
                     return (
                        <div key={mId} className="border border-slate-300 p-3 rounded-lg flex items-center justify-between break-inside-avoid">
                           <div className="flex items-center gap-3">
                              <div className="w-8 h-8 bg-red-600 text-white flex items-center justify-center font-black rounded text-xs">{mealNames[mId].substring(0,2).toUpperCase()}</div>
                              <div>
                                 <div className="text-sm font-bold uppercase">{p.item?.label || 'None'}</div>
                                 <div className="text-[10px] text-slate-500 uppercase font-bold">
                                    {program.phase !== 'm1-deplete' && `+ ${c.item?.label || 'None'}`} {`+ ${f.item?.label || 'None'}`}
                                 </div>
                              </div>
                           </div>
                           <div className="text-right text-xs font-mono">
                              <div className="font-bold">{p.val}{p.unit}</div>
                              {program.phase !== 'm1-deplete' && <div>{c.val}{c.unit}</div>}
                              <div>{f.val}{f.unit}</div>
                           </div>
                        </div>
                     );
                  })}
               </div>
            </div>

            {/* Right: Grocery List & Stats */}
            <div>
               <div className="mb-8 break-inside-avoid">
                  <h3 className="font-black uppercase text-lg border-b-2 border-black mb-4">{shoppingDays}-Day Shopping List</h3>
                  <div className="bg-slate-100 p-4 rounded-lg">
                     <div className="grid grid-cols-2 gap-4 text-xs">
                        {Object.entries(groceryList).map(([name, amount]) => (
                           <div key={name} className="flex justify-between border-b border-slate-300 pb-1">
                              <span className="font-bold uppercase text-[10px] truncate pr-2">{name}</span>
                              <span className="font-mono font-bold text-red-700 whitespace-nowrap">{amount}</span>
                           </div>
                        ))}
                     </div>
                  </div>
               </div>

               <div className="break-inside-avoid">
                  <h3 className="font-black uppercase text-lg border-b-2 border-black mb-4">Daily Targets</h3>
                  <div className="grid grid-cols-4 gap-2 text-center">
                     <div className="border border-slate-300 p-2 rounded">
                        <div className="text-[10px] font-bold text-slate-500">CALORIES</div>
                        <div className="font-black text-lg">{dailyCals.toFixed(0)}</div>
                     </div>
                     <div className="border border-slate-300 p-2 rounded">
                        <div className="text-[10px] font-bold text-slate-500">PROTEIN</div>
                        <div className="font-black text-lg">{dailyP.toFixed(0)}g</div>
                     </div>
                     <div className="border border-slate-300 p-2 rounded">
                        <div className="text-[10px] font-bold text-slate-500">CARBS</div>
                        <div className="font-black text-lg">{program.phase==='m1-deplete'?'<30':dailyC.toFixed(0)}g</div>
                     </div>
                     <div className="border border-slate-300 p-2 rounded">
                        <div className="text-[10px] font-bold text-slate-500">FATS</div>
                        <div className="font-black text-lg">{dailyF.toFixed(0)}g</div>
                     </div>
                  </div>
               </div>
            </div>
         </div>

         {/* UNLIMITED VEG & CONDIMENTS */}
         <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8 break-inside-avoid">
            <div className="border border-slate-700 p-4 print:border-2 print:border-black">
               <h3 className="font-bold text-white uppercase mb-2 flex items-center print:text-black text-xs border-b border-slate-800 print:border-black pb-1"><Leaf className="w-3 h-3 mr-2 text-emerald-500 print:text-black" /> Unlimited Vegetables</h3>
               <div className="text-[10px] text-slate-400 print:text-slate-600 leading-relaxed">{UNLIMITED_VEG.join(", ")}</div>
            </div>
            <div className="border border-slate-700 p-4 print:border-2 print:border-black">
               <h3 className="font-bold text-white uppercase mb-2 flex items-center print:text-black text-xs border-b border-slate-800 print:border-black pb-1"><Info className="w-3 h-3 mr-2 text-blue-500 print:text-black" /> Free Condiments</h3>
               <div className="text-[10px] text-slate-400 print:text-slate-600 leading-relaxed">{FREE_CONDIMENTS.join(", ")}</div>
            </div>
         </div>

         {/* LIFESTYLE & EATING OUT */}
         <div className="grid grid-cols-2 gap-8 mb-8 border-t-2 border-black pt-8 break-inside-avoid">
            <div>
               <h3 className="font-black uppercase text-sm border-b-2 border-black mb-4">Weekly Lifestyle Targets</h3>
               <ul className="text-xs space-y-2">
                  <li className="flex justify-between border-b border-slate-300 pb-1"><span>Sleep</span> <span className="font-bold">7.5+ Hours</span></li>
                  <li className="flex justify-between border-b border-slate-300 pb-1"><span>Hydration</span> <span className="font-bold">{dailyWater.toFixed(0)}oz Water</span></li>
                  <li className="flex justify-between border-b border-slate-300 pb-1"><span>Movement</span> <span className="font-bold">8,000 Steps</span></li>
                  <li className="flex justify-between border-b border-slate-300 pb-1"><span>Training</span> <span className="font-bold">4 Sessions/Wk</span></li>
               </ul>
            </div>
            <div>
               <h3 className="font-black uppercase text-sm border-b-2 border-black mb-4">Approved Eating Out Guide</h3>
               <div className="text-[10px] space-y-2 text-slate-700">
                  <p><strong>• Chipotle:</strong> Salad Bowl, Double Chicken, Mild Salsa, Guac. No Rice/Beans.</p>
                  <p><strong>• Burger Joint:</strong> Burger (No Bun), Side Salad (Dressing on Side) or Steamed Broccoli.</p>
                  <p><strong>• Sushi:</strong> Sashimi (Fish only), Edamame. No Rolls/Rice.</p>
                  <p><strong>• Steakhouse:</strong> 6oz Sirloin/Filet, Asparagus/Broccoli. No Bread Basket.</p>
               </div>
            </div>
         </div>

         {/* SUPPLEMENT STACK SECTION */}
         <div className="hidden print:block border-t-2 border-black pt-8 mb-8 break-inside-avoid">
            <h3 className="font-black uppercase text-lg border-b-2 border-black mb-4">MTS Vitality Stack</h3>
            <div className="grid grid-cols-3 gap-6">
               <div>
                   <div className="font-bold text-sm uppercase mb-1">Morning</div>
                   <ul className="text-xs text-slate-600 space-y-1">
                       <li>• Multi-Vitamin</li>
                       <li>• Vitamin D3 (5000 IU)</li>
                       <li>• Omega-3 Fish Oil (2g)</li>
                   </ul>
               </div>
               <div>
                   <div className="font-bold text-sm uppercase mb-1">Pre/Intra Workout</div>
                   <ul className="text-xs text-slate-600 space-y-1">
                       <li>• Essential Aminos (EAAs)</li>
                       <li>• Creatine Monohydrate (5g)</li>
                       <li>• Himalayan Salt (1/2 tsp)</li>
                   </ul>
               </div>
               <div>
                   <div className="font-bold text-sm uppercase mb-1">Evening/Recovery</div>
                   <ul className="text-xs text-slate-600 space-y-1">
                       <li>• Magnesium Glycinate (400mg)</li>
                       <li>• Zinc (30mg)</li>
                   </ul>
               </div>
            </div>
         </div>

         {/* EMERGENCY MEAL OPTION */}
         <div className="hidden print:block bg-slate-100 p-4 border-2 border-black border-dashed mb-8 break-inside-avoid">
            <h3 className="font-black uppercase text-xs mb-2">🚨 Emergency "Panic Button" Option</h3>
            <p className="text-xs text-slate-600">If you are stuck without prep: <strong>Rotisserie Chicken (Skin off) + Bag of Steamed Broccoli</strong>. Available at any grocery store. No excuses.</p>
         </div>

         {/* THE CONTRACT - FOOTER */}
         <div className="mt-12 pt-8 border-t-4 border-black break-inside-avoid">
            <div className="flex justify-between items-end mb-8">
               <div className="text-center w-1/3">
                  <div className="border-b border-black h-8"></div>
                  <div className="text-[10px] font-bold uppercase mt-1">Coach Signature</div>
               </div>
               <div className="text-center w-1/3 px-4">
                  <div className="border-2 border-red-600 text-red-600 p-2 font-black text-xs uppercase rounded transform -rotate-2">
                     Official Centerville Protocol
                  </div>
               </div>
               <div className="text-center w-1/3">
                  <div className="border-b border-black h-8"></div>
                  <div className="text-[10px] font-bold uppercase mt-1">Client Commitment</div>
               </div>
            </div>
            
            <div className="flex justify-between text-[10px] text-slate-500 uppercase font-bold tracking-widest">
               <div>Effective Date: {new Date().toLocaleDateString()}</div>
               <div>Next Appointment: _______________________</div>
            </div>
         </div>
      </div>
      
      <style>{`
        @media print {
          @page { size: portrait; margin: 0.25in; }
          body { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
          .break-inside-avoid { break-inside: avoid; }
        }
        .custom-scrollbar::-webkit-scrollbar {
          width: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
          background: rgba(0,0,0,0.2);
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
          background: #10b981;
          border-radius: 4px;
        }
      `}</style>

    </div>
  );
}
