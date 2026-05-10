/**
 * @license
 * SPDX-License-Identifier: Apache-2.0
 */

import React, { useState } from 'react';
import { 
  Calculator, 
  Zap, 
  MoveRight, 
  Activity, 
  BookOpen, 
  Info,
  RefreshCcw,
  LayoutDashboard
} from 'lucide-react';
import { motion, AnimatePresence } from 'motion/react';

// SciInput component for scientific notation input (a × 10^n)
const SciInput = ({ label, base, exp, onBaseChange, onExpChange, unit, colorClass = "blue" }: {
  label: string;
  base: string;
  exp: string;
  onBaseChange: (val: string) => void;
  onExpChange: (val: string) => void;
  unit: string;
  colorClass?: string;
}) => (
  <div className="flex flex-col mb-6">
    <label className="text-sm font-bold text-gray-600 mb-2">{label}</label>
    <div className="flex items-center gap-3">
      <div className="relative flex-1">
        <input
          type="number"
          value={base}
          onChange={(e) => onBaseChange(e.target.value)}
          className={`w-full p-2.5 bg-white border border-gray-200 rounded-lg shadow-sm focus:ring-2 focus:ring-${colorClass}-500 focus:border-transparent outline-none transition-all`}
          placeholder="មេគុណ (a)"
        />
      </div>
      <span className="text-gray-400 font-bold italic">× 10</span>
      <div className="relative w-24">
        <input
          type="number"
          value={exp}
          onChange={(e) => onExpChange(e.target.value)}
          className={`w-full p-2.5 bg-white border border-gray-200 rounded-lg shadow-sm focus:ring-2 focus:ring-${colorClass}-500 focus:border-transparent outline-none transition-all`}
          placeholder="ស្វ័យគុណ (n)"
        />
      </div>
      <div className="min-w-[40px] font-bold text-gray-500 bg-gray-100 px-3 py-2 rounded-lg text-center">
        {unit}
      </div>
    </div>
  </div>
);

// Coulomb's Law Tab
const CoulombTab = () => {
  const [q1Base, setQ1Base] = useState('2');
  const [q1Exp, setQ1Exp] = useState('-6');
  const [q2Base, setQ2Base] = useState('3');
  const [q2Exp, setQ2Exp] = useState('-6');
  const [rBase, setRBase] = useState('0.1');
  const [rExp, setRExp] = useState('0');

  const calculateF = () => {
    const k = 9e9;
    const q1 = parseFloat(q1Base) * Math.pow(10, parseFloat(q1Exp));
    const q2 = parseFloat(q2Base) * Math.pow(10, parseFloat(q2Exp));
    const r = parseFloat(rBase) * Math.pow(10, parseFloat(rExp));
    
    if (r === 0 || isNaN(q1) || isNaN(q2) || isNaN(r)) return '---';
    
    const f = (k * Math.abs(q1 * q2)) / Math.pow(r, 2);
    return f.toExponential(3);
  };

  return (
    <motion.div 
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      className="p-4 md:p-8"
    >
      <div className="flex items-center gap-3 mb-6">
        <div className="p-3 bg-blue-100 rounded-xl">
          <Zap className="text-blue-600" size={24} />
        </div>
        <div>
          <h3 className="text-xl font-bold text-gray-800">ច្បាប់គូឡុំ (Coulomb's Law)</h3>
          <p className="text-sm text-gray-500">គណនាកម្លាំងអន្តរកម្មរវាងពីរបន្ទុកចំណុច</p>
        </div>
      </div>

      <div className="bg-blue-50/50 p-4 rounded-xl border border-blue-100 mb-8 flex items-start gap-3">
        <Info className="text-blue-400 mt-0.5" size={18} />
        <div className="text-sm text-blue-800">
          <span className="font-bold">រូបមន្ត៖</span> F = k × |q₁q₂| / r² (ដែល k = 9 × 10⁹ N·m²/C²)
        </div>
      </div>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 items-start">
        <div className="space-y-2">
          <SciInput label="បន្ទុកអគ្គិសនីទី១ (q₁)" base={q1Base} exp={q1Exp} onBaseChange={setQ1Base} onExpChange={setQ1Exp} unit="C" colorClass="blue" />
          <SciInput label="បន្ទុកអគ្គិសនីទី២ (q₂)" base={q2Base} exp={q2Exp} onBaseChange={setQ2Base} onExpChange={setQ2Exp} unit="C" colorClass="blue" />
          <SciInput label="ចម្ងាយរវាងបន្ទុកទាំងពីរ (r)" base={rBase} exp={rExp} onBaseChange={setRBase} onExpChange={setRExp} unit="m" colorClass="blue" />
        </div>
        
        <div className="flex flex-col h-full justify-between">
          <div className="bg-gradient-to-br from-blue-600 to-indigo-700 rounded-3xl p-8 text-white shadow-xl flex flex-col items-center justify-center text-center">
            <div className="text-blue-100 text-sm font-medium mb-4 uppercase tracking-wider">កម្លាំងអគ្គិសនី (F)</div>
            <motion.div 
              key={calculateF()}
              initial={{ scale: 0.95, opacity: 0 }}
              animate={{ scale: 1, opacity: 1 }}
              className="text-4xl md:text-5xl font-black mb-2"
            >
              {calculateF()}
            </motion.div>
            <div className="text-2xl font-bold text-blue-200">ញូតុន (N)</div>
          </div>
          
          <div className="mt-6 flex gap-4">
            <button 
              onClick={() => {
                setQ1Base('2'); setQ1Exp('-6');
                setQ2Base('3'); setQ2Exp('-6');
                setRBase('0.1'); setRExp('0');
              }}
              className="flex-1 py-3 px-4 rounded-xl border border-gray-200 text-gray-600 font-bold hover:bg-gray-50 flex items-center justify-center gap-2 transition-colors"
            >
              <RefreshCcw size={18} /> កំណត់ឡើងវិញ
            </button>
          </div>
        </div>
      </div>
    </motion.div>
  );
};

// Electric Field Tab
const EFieldTab = () => {
  const [qBase, setQBase] = useState('2');
  const [qExp, setQExp] = useState('-6');
  const [rBase, setRBase] = useState('0.1');
  const [rExp, setRExp] = useState('0');

  const calculateE = () => {
    const k = 9e9;
    const q = parseFloat(qBase) * Math.pow(10, parseFloat(qExp));
    const r = parseFloat(rBase) * Math.pow(10, parseFloat(rExp));
    
    if (r === 0 || isNaN(q) || isNaN(r)) return '---';
    
    const e = (k * Math.abs(q)) / Math.pow(r, 2);
    return e.toExponential(3);
  };

  return (
    <motion.div 
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      className="p-4 md:p-8"
    >
      <div className="flex items-center gap-3 mb-6">
        <div className="p-3 bg-purple-100 rounded-xl">
          <Activity className="text-purple-600" size={24} />
        </div>
        <div>
          <h3 className="text-xl font-bold text-gray-800">ដែនអគ្គិសនី (Electric Field)</h3>
          <p className="text-sm text-gray-500">គណនាខ្លឹមភាពដែនអគ្គិសនីត្រង់ចំណុចមួយ</p>
        </div>
      </div>

      <div className="bg-purple-50/50 p-4 rounded-xl border border-purple-100 mb-8 flex items-start gap-3">
        <Info className="text-purple-400 mt-0.5" size={18} />
        <div className="text-sm text-purple-800">
          <span className="font-bold">រូបមន្ត៖</span> E = k × |Q| / r²
        </div>
      </div>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 items-start">
        <div className="space-y-2">
          <SciInput label="បន្ទុកបង្កដែន (Q)" base={qBase} exp={qExp} onBaseChange={setQBase} onExpChange={setQExp} unit="C" colorClass="purple" />
          <SciInput label="ចម្ងាយពីបន្ទុកទៅចំណុចគណនា (r)" base={rBase} exp={rExp} onBaseChange={setRBase} onExpChange={setRExp} unit="m" colorClass="purple" />
        </div>
        
        <div className="flex flex-col h-full">
          <div className="bg-gradient-to-br from-purple-600 to-pink-700 rounded-3xl p-8 text-white shadow-xl flex flex-col items-center justify-center text-center">
            <div className="text-purple-100 text-sm font-medium mb-4 uppercase tracking-wider">ខ្លឹមភាពដែនអគ្គិសនី (E)</div>
            <motion.div 
              key={calculateE()}
              initial={{ scale: 0.95, opacity: 0 }}
              animate={{ scale: 1, opacity: 1 }}
              className="text-4xl md:text-5xl font-black mb-2"
            >
              {calculateE()}
            </motion.div>
            <div className="text-2xl font-bold text-purple-200">N/C</div>
          </div>
        </div>
      </div>
    </motion.div>
  );
};

// Motion of Particles Tab
const MotionTab = () => {
  const [qBase, setQBase] = useState('2');
  const [qExp, setQExp] = useState('-6');
  const [eBase, setEBase] = useState('5');
  const [eExp, setEExp] = useState('3');
  const [mBase, setMBase] = useState('4');
  const [mExp, setMExp] = useState('-3');

  const calculateA = () => {
    const q = parseFloat(qBase) * Math.pow(10, parseFloat(qExp));
    const e = parseFloat(eBase) * Math.pow(10, parseFloat(eExp));
    const m = parseFloat(mBase) * Math.pow(10, parseFloat(mExp));
    
    if (m <= 0 || isNaN(q) || isNaN(e) || isNaN(m)) return '---';
    
    const a = (Math.abs(q) * e) / m;
    return a.toExponential(3);
  };

  return (
    <motion.div 
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      className="p-4 md:p-8"
    >
      <div className="flex items-center gap-3 mb-6">
        <div className="p-3 bg-emerald-100 rounded-xl">
          <MoveRight className="text-emerald-600" size={24} />
        </div>
        <div>
          <h3 className="text-xl font-bold text-gray-800">ចលនាភាគល្អិតអគ្គិសនី (Particle Motion)</h3>
          <p className="text-sm text-gray-500">គណនាសំទុះរបស់ភាគល្អិតក្នុងដែនអគ្គិសនីឯកសណ្ឋាន</p>
        </div>
      </div>

      <div className="bg-emerald-50/50 p-4 rounded-xl border border-emerald-100 mb-8 flex items-start gap-3">
        <Info className="text-emerald-400 mt-0.5" size={18} />
        <div className="text-sm text-emerald-800">
          <span className="font-bold">រូបមន្ត៖</span> a = |q|E / m
        </div>
      </div>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 items-start">
        <div className="space-y-2">
          <SciInput label="បន្ទុកភាគល្អិត (q)" base={qBase} exp={qExp} onBaseChange={setQBase} onExpChange={setQExp} unit="C" colorClass="emerald" />
          <SciInput label="ខ្លឹមភាពដែនអគ្គិសនី (E)" base={eBase} exp={eExp} onBaseChange={setEBase} onExpChange={setEExp} unit="N/C" colorClass="emerald" />
          <SciInput label="ម៉ាសភាគល្អិត (m)" base={mBase} exp={mExp} onBaseChange={setMBase} onExpChange={setMExp} unit="kg" colorClass="emerald" />
        </div>
        
        <div className="flex flex-col h-full">
          <div className="bg-gradient-to-br from-emerald-600 to-teal-700 rounded-3xl p-8 text-white shadow-xl flex flex-col items-center justify-center text-center">
            <div className="text-emerald-100 text-sm font-medium mb-4 uppercase tracking-wider">សំទុះរបស់ភាគល្អិត (a)</div>
            <motion.div 
              key={calculateA()}
              initial={{ scale: 0.95, opacity: 0 }}
              animate={{ scale: 1, opacity: 1 }}
              className="text-4xl md:text-5xl font-black mb-2"
            >
              {calculateA()}
            </motion.div>
            <div className="text-2xl font-bold text-emerald-200">m/s²</div>
          </div>
        </div>
      </div>
    </motion.div>
  );
};

// Theory/Lesson Content Tab
const TheoryTab = ({ content, onUpdate }: { content: any, onUpdate: (newContent: any) => void }) => {
  const [isEditing, setIsEditing] = useState(false);
  const [formData, setFormData] = useState(content);

  const handleSave = () => {
    onUpdate(formData);
    setIsEditing(false);
  };

  if (isEditing) {
    return (
      <motion.div 
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        className="p-4 md:p-8"
      >
        <div className="flex items-center justify-between mb-8">
          <h3 className="text-xl font-bold text-gray-800">កែសម្រួលខ្លឹមសារមេរៀន</h3>
          <div className="flex gap-2">
            <button 
              onClick={() => setIsEditing(false)}
              className="px-4 py-2 text-sm font-bold text-gray-500 hover:bg-gray-100 rounded-lg transition-colors"
            >
              បោះបង់
            </button>
            <button 
              onClick={handleSave}
              className="px-4 py-2 text-sm font-bold bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors shadow-lg shadow-blue-200"
            >
              រក្សាទុក
            </button>
          </div>
        </div>

        <div className="space-y-8">
          <div className="bg-slate-50 p-6 rounded-2xl border border-slate-200">
            <h4 className="font-bold text-blue-700 mb-4 italic underline">ផ្នែកទី១ (ច្បាប់គូឡុំ)</h4>
            <div className="space-y-4">
              <input 
                className="w-full p-2 border rounded-lg" 
                value={formData.section1.title} 
                onChange={e => setFormData({...formData, section1: {...formData.section1, title: e.target.value}})}
                placeholder="ចំណងជើង"
              />
              <textarea 
                className="w-full p-2 border rounded-lg h-24" 
                value={formData.section1.desc} 
                onChange={e => setFormData({...formData, section1: {...formData.section1, desc: e.target.value}})}
                placeholder="ខ្លឹមសារ"
              />
              <input 
                className="w-full p-2 border rounded-lg font-mono" 
                value={formData.section1.formula} 
                onChange={e => setFormData({...formData, section1: {...formData.section1, formula: e.target.value}})}
                placeholder="រូបមន្ត"
              />
            </div>
          </div>

          <div className="bg-slate-50 p-6 rounded-2xl border border-slate-200">
            <h4 className="font-bold text-purple-700 mb-4 italic underline">ផ្នែកទី២ (ដែនអគ្គិសនី)</h4>
            <div className="space-y-4">
              <input 
                className="w-full p-2 border rounded-lg" 
                value={formData.section2.title} 
                onChange={e => setFormData({...formData, section2: {...formData.section2, title: e.target.value}})}
              />
              <textarea 
                className="w-full p-2 border rounded-lg h-24" 
                value={formData.section2.desc} 
                onChange={e => setFormData({...formData, section2: {...formData.section2, desc: e.target.value}})}
              />
              <input 
                className="w-full p-2 border rounded-lg font-mono" 
                value={formData.section2.formula} 
                onChange={e => setFormData({...formData, section2: {...formData.section2, formula: e.target.value}})}
              />
            </div>
          </div>
        </div>
      </motion.div>
    );
  }

  return (
    <motion.div 
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      className="p-4 md:p-8"
    >
      <div className="flex items-center justify-between mb-8">
        <div className="flex items-center gap-3">
          <div className="p-3 bg-amber-100 rounded-xl">
            <BookOpen className="text-amber-600" size={24} />
          </div>
          <div>
            <h3 className="text-xl font-bold text-gray-800">ខ្លឹមសារសង្ខេបមេរៀន (Lesson Summary)</h3>
            <p className="text-sm text-gray-500">មេរៀនទី១៖ បន្ទុក និងដែនអគ្គិសនី</p>
          </div>
        </div>
        <button 
          onClick={() => { setFormData(content); setIsEditing(true); }}
          className="flex items-center gap-2 px-4 py-2 bg-slate-100 text-slate-600 rounded-xl hover:bg-slate-200 font-bold transition-all text-sm"
        >
          <RefreshCcw size={16} /> កែសម្រួល
        </button>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        {/* Section 1 */}
        <div className="bg-slate-50 p-6 rounded-2xl border border-slate-200">
          <h4 className="font-bold text-blue-700 flex items-center gap-2 mb-4">
            <Zap size={18} /> {content.section1.title}
          </h4>
          <p className="text-sm leading-relaxed text-gray-700 mb-4">
            {content.section1.desc}
          </p>
          <div className="bg-white p-3 rounded-xl border border-slate-100 font-mono text-center text-blue-600 font-bold">
            {content.section1.formula}
          </div>
        </div>

        {/* Section 2 */}
        <div className="bg-slate-50 p-6 rounded-2xl border border-slate-200">
          <h4 className="font-bold text-purple-700 flex items-center gap-2 mb-4">
            <Activity size={18} /> {content.section2.title}
          </h4>
          <p className="text-sm leading-relaxed text-gray-700 mb-4">
            {content.section2.desc}
          </p>
          <div className="bg-white p-3 rounded-xl border border-slate-100 font-mono text-center text-purple-600 font-bold">
            {content.section2.formula}
          </div>
        </div>

        {/* Static Section for Motion */}
        <div className="bg-slate-50 p-6 rounded-2xl border border-slate-200 md:col-span-2">
          <h4 className="font-bold text-emerald-700 flex items-center gap-2 mb-4">
            <MoveRight size={18} /> ៣. ចលនាភាគល្អិតក្នុងដែនអគ្គិសនី
          </h4>
          <div className="grid grid-cols-1 sm:grid-cols-2 gap-6">
            <div>
              <p className="text-sm text-gray-700 mb-3">នៅពេលភាគល្អិតបន្ទុក q ស្ថិតក្នុងដែនអគ្គិសនី E វារងនូវកម្លាំង៖</p>
              <div className="bg-white p-3 rounded-xl border border-slate-100 font-mono text-center text-emerald-600 font-bold mb-2">
                F = q · E
              </div>
            </div>
            <div>
              <p className="text-sm text-gray-700 mb-3">តាមច្បាប់ទី២ញូតុន (F = m·a) យើងទាញបានសំទុះ៖</p>
              <div className="bg-white p-3 rounded-xl border border-slate-100 font-mono text-center text-emerald-600 font-bold mb-2">
                a = |q|E / m
              </div>
            </div>
          </div>
        </div>
      </div>
    </motion.div>
  );
};

export default function App() {
  const [activeTab, setActiveTab] = useState('theory');
  const [lessonContent, setLessonContent] = useState({
    section1: {
      title: "១. ច្បាប់គូឡុំ (Coulomb's Law)",
      desc: "កម្លាំងអន្តរកម្មរវាងពីរបន្ទុកចំណុចសមាមាត្រនឹងផលគុណនៃបន្ទុកទាំងពីរ និងច្រាសសមាមាត្រនឹងការរេនៃចម្ងាយរវាងបន្ទុកទាំងនោះ។",
      formula: "F = k · |q₁ · q₂| / r²"
    },
    section2: {
      title: "២. ដែនអគ្គិសនី (Electric Field)",
      desc: "ជាលំហដែលព័ទ្ធជុំវិញបន្ទុកអគ្គិសនី ហើយផ្តល់នូវកម្លាំងអគ្គិសនីទៅលើបន្ទុកផ្សេងទៀតដែលដាក់ក្នុងលំហនោះ។",
      formula: "E = k · |Q| / r²"
    }
  });

  const tabs = [
    { id: 'theory', label: 'ខ្លឹមសារមេរៀន', icon: BookOpen, color: 'amber' },
    { id: 'coulomb', label: 'ច្បាប់គូឡុំ', icon: Zap, color: 'blue' },
    { id: 'efield', label: 'ដែនអគ្គិសនី', icon: Activity, color: 'purple' },
    { id: 'motion', label: 'ចលនាភាគល្អិត', icon: MoveRight, color: 'emerald' },
  ];

  return (
    <div className="min-h-screen bg-[#f8fafc] flex flex-col">
      {/* Top Header Section */}
      <header className="bg-slate-900 text-white pt-12 pb-24 relative overflow-hidden">
        {/* Animated Background Element */}
        <div className="absolute top-0 right-0 w-96 h-96 bg-blue-500/10 rounded-full blur-3xl -mr-20 -mt-20" />
        <div className="absolute bottom-0 left-0 w-64 h-64 bg-indigo-500/10 rounded-full blur-2xl -ml-20 -mb-20" />
        
        <div className="max-w-5xl mx-auto px-4 relative z-10">
          <motion.div 
            initial={{ opacity: 0, y: -20 }}
            animate={{ opacity: 1, y: 0 }}
            className="flex flex-col md:flex-row md:items-center justify-between gap-6"
          >
            <div className="flex items-center gap-5">
              <div className="w-16 h-16 bg-blue-600 rounded-2xl flex items-center justify-center shadow-2xl shadow-blue-500/40">
                <Calculator size={34} className="text-white" />
              </div>
              <div>
                <h1 className="text-3xl md:text-4xl font-bold font-moul leading-tight">កម្មវិធីជំនួយការគណនារូបវិទ្យា</h1>
                <div className="flex items-center gap-2 text-blue-300 mt-2 font-medium">
                  <LayoutDashboard size={16} />
                  <span>ថ្នាក់ទី១១ ជំពូកទី៤ មេរៀនទី១៖ បន្ទុក និងដែនអគ្គិសនី</span>
                </div>
              </div>
            </div>
            
            <div className="hidden md:flex flex-col items-end border-l border-slate-700 pl-8">
              <span className="text-slate-400 text-xs uppercase tracking-[0.2em] mb-1 font-bold">ផលិតដោយគ្រូបង្រៀន</span>
              <span className="text-xl font-bold text-white">លោកគ្រូ ឡាង ចាន់ម៉ូលី</span>
            </div>
          </motion.div>
        </div>
      </header>

      {/* Main Content Area */}
      <main className="max-w-5xl mx-auto w-full px-4 -mt-12 mb-12 relative z-20">
        <div className="bg-white rounded-[2rem] shadow-2xl shadow-slate-200 overflow-hidden border border-slate-100">
          {/* Navigation Bar */}
          <nav className="flex flex-wrap p-2 bg-slate-50/50 border-b border-slate-100">
            {tabs.map((tab) => {
              const isActive = activeTab === tab.id;
              const Icon = tab.icon;
              return (
                <button
                  key={tab.id}
                  onClick={() => setActiveTab(tab.id)}
                  className={`flex items-center gap-3 px-6 py-4 rounded-2xl font-bold transition-all flex-1 md:flex-none justify-center whitespace-nowrap ${
                    isActive 
                      ? 'bg-white text-slate-900 shadow-sm ring-1 ring-slate-200' 
                      : 'text-slate-500 hover:text-slate-700 hover:bg-slate-100/50'
                  }`}
                >
                  <Icon size={20} className={isActive ? `text-${tab.color}-600` : ''} />
                  {tab.label}
                </button>
              );
            })}
          </nav>

          {/* Active Tab Content */}
          <div className="min-h-[400px]">
            <AnimatePresence mode="wait">
              {activeTab === 'theory' && <TheoryTab key="theory" content={lessonContent} onUpdate={setLessonContent} />}
              {activeTab === 'coulomb' && <CoulombTab key="coulomb" />}
              {activeTab === 'efield' && <EFieldTab key="efield" />}
              {activeTab === 'motion' && <MotionTab key="motion" />}
            </AnimatePresence>
          </div>

          {/* Footer Info Box */}
          <footer className="bg-slate-50 border-t border-gray-100 px-8 py-6">
            <div className="flex flex-col md:flex-row md:items-center justify-between gap-4">
              <div className="flex items-center gap-3">
                <BookOpen size={18} className="text-slate-400" />
                <span className="text-sm text-slate-500 font-medium">
                  សម្រួលដល់ការសិក្សាស្រាវជ្រាវ និងការបង្រៀនរូបវិទ្យា
                </span>
              </div>
              <div className="flex items-center gap-2 py-1.5 px-4 bg-white rounded-full border border-slate-200 shadow-sm">
                <div className="w-2 h-2 bg-green-500 rounded-full animate-pulse" />
                <span className="text-xs font-bold text-slate-700 tracking-wide">កំណែ v1.0.0</span>
              </div>
            </div>
          </footer>
        </div>
      </main>
      
      {/* Signature Section (Mobile Only) */}
      <div className="md:hidden text-center pb-8 px-4">
        <p className="text-slate-400 text-xs font-bold uppercase tracking-widest mb-1">រៀបចំដោយ</p>
        <p className="text-slate-700 font-bold">លោកគ្រូ ឡាង ចាន់ម៉ូលី</p>
      </div>
    </div>
  );
}
