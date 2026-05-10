<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>កម្មវិធីជំនួយការគណនារូបវិទ្យា - ឡាង ចាន់ម៉ូលី</title>
    
    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Kantumruy+Pro:wght@300;400;500;600;700&family=Moul&display=swap" rel="stylesheet">
    
    <!-- Scripts -->
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <!-- Framer Motion (Animation) -->
    <script src="https://unpkg.com/framer-motion@10.16.4/dist/framer-motion.js"></script>

    <style>
        :root {
            --font-sans: 'Kantumruy Pro', sans-serif;
            --font-moul: 'Moul', serif;
        }
        body {
            font-family: var(--font-sans);
        }
        .font-moul {
            font-family: var(--font-moul);
        }
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
    </style>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Kantumruy Pro', 'sans-serif'],
                        moul: ['Moul', 'serif'],
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-slate-50 text-slate-900">
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;
        const { motion, AnimatePresence } = window.framerMotion;

        // Custom Icon Component using Lucide CDN
        const Icon = ({ name, size = 20, className = "" }) => {
            useEffect(() => {
                if (window.lucide) {
                    window.lucide.createIcons();
                }
            }, [name]);

            return <i data-lucide={name} style={{width: size, height: size}} className={`inline-block ${className}`}></i>;
        };

        const SciInput = ({ label, base, exp, onBaseChange, onExpChange, unit, colorClass = "blue" }) => (
            <div className="flex flex-col mb-6">
                <label className="text-sm font-bold text-gray-600 mb-2">{label}</label>
                <div className="flex items-center gap-3">
                    <div className="relative flex-1">
                        <input
                            type="number"
                            value={base}
                            onChange={(e) => onBaseChange(e.target.value)}
                            className={`w-full p-2.5 bg-white border border-gray-200 rounded-lg shadow-sm focus:ring-2 focus:ring-${colorClass}-500 focus:border-transparent outline-none transition-all`}
                            placeholder="a"
                        />
                    </div>
                    <span className="text-gray-400 font-bold italic">× 10</span>
                    <div className="relative w-24">
                        <input
                            type="number"
                            value={exp}
                            onChange={(e) => onExpChange(e.target.value)}
                            className={`w-full p-2.5 bg-white border border-gray-200 rounded-lg shadow-sm focus:ring-2 focus:ring-${colorClass}-500 focus:border-transparent outline-none transition-all`}
                            placeholder="n"
                        />
                    </div>
                    <div className="min-w-[40px] font-bold text-gray-500 bg-gray-100 px-3 py-2 rounded-lg text-center">
                        {unit}
                    </div>
                </div>
            </div>
        );

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
                <motion.div initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} className="p-4 md:p-8">
                    <div className="flex items-center gap-3 mb-6">
                        <div className="p-3 bg-blue-100 rounded-xl">
                            <Icon name="zap" className="text-blue-600" size={24} />
                        </div>
                        <div>
                            <h3 className="text-xl font-bold">ច្បាប់គូឡុំ (Coulomb's Law)</h3>
                            <p className="text-sm text-gray-500">គណនាកម្លាំងអន្តរកម្មរវាងពីរបន្ទុកចំណុច</p>
                        </div>
                    </div>
                    <div className="bg-blue-50/50 p-4 rounded-xl border border-blue-100 mb-8 flex items-start gap-3">
                        <Icon name="info" className="text-blue-400 mt-0.5" size={18} />
                        <div className="text-sm text-blue-800">
                            <span className="font-bold">រូបមន្ត៖</span> F = k × |q₁q₂| / r² (ដែល k = 9 × 10⁹ N·m²/C²)
                        </div>
                    </div>
                    <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
                        <div>
                            <SciInput label="បន្ទុកអគ្គិសនីទី១ (q₁)" base={q1Base} exp={q1Exp} onBaseChange={setQ1Base} onExpChange={setQ1Exp} unit="C" />
                            <SciInput label="បន្ទុកអគ្គិសនីទី២ (q₂)" base={q2Base} exp={q2Exp} onBaseChange={setQ2Base} onExpChange={setQ2Exp} unit="C" />
                            <SciInput label="ចម្ងាយរវាងបន្ទុកទាំងពីរ (r)" base={rBase} exp={rExp} onBaseChange={setRBase} onExpChange={setRExp} unit="m" />
                        </div>
                        <div className="flex flex-col h-full justify-center">
                            <div className="bg-gradient-to-br from-blue-600 to-indigo-700 rounded-3xl p-8 text-white shadow-xl text-center">
                                <div className="text-blue-100 text-sm font-medium mb-4 uppercase">កម្លាំងអគ្គិសនី (F)</div>
                                <div className="text-4xl md:text-5xl font-black mb-2">{calculateF()}</div>
                                <div className="text-2xl font-bold text-blue-200">ញូតុន (N)</div>
                            </div>
                        </div>
                    </div>
                </motion.div>
            );
        };

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
                <motion.div initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} className="p-4 md:p-8">
                    <div className="flex items-center gap-3 mb-6">
                        <div className="p-3 bg-purple-100 rounded-xl">
                            <Icon name="activity" className="text-purple-600" size={24} />
                        </div>
                        <div>
                            <h3 className="text-xl font-bold">ដែនអគ្គិសនី (Electric Field)</h3>
                            <p className="text-sm text-gray-500">គណនាខ្លឹមភាពដែនអគ្គិសនីត្រង់ចំណុចមួយ</p>
                        </div>
                    </div>
                    <div className="bg-purple-50/50 p-4 rounded-xl border border-purple-100 mb-8 flex items-start gap-3">
                        <Icon name="info" className="text-purple-400 mt-0.5" size={18} />
                        <div className="text-sm text-purple-800">
                            <span className="font-bold">រូបមន្ត៖</span> E = k × |Q| / r²
                        </div>
                    </div>
                    <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
                        <div>
                            <SciInput label="បន្ទុកបង្កដែន (Q)" base={qBase} exp={qExp} onBaseChange={setQBase} onExpChange={setQExp} unit="C" colorClass="purple" />
                            <SciInput label="ចម្ងាយពីបន្ទុកទៅចំណុចគណនា (r)" base={rBase} exp={rExp} onBaseChange={setRBase} onExpChange={setRExp} unit="m" colorClass="purple" />
                        </div>
                        <div className="flex flex-col h-full justify-center">
                            <div className="bg-gradient-to-br from-purple-600 to-pink-700 rounded-3xl p-8 text-white shadow-xl text-center">
                                <div className="text-purple-100 text-sm font-medium mb-4 uppercase">ខ្លឹមភាពដែនអគ្គិសនី (E)</div>
                                <div className="text-4xl md:text-5xl font-black mb-2">{calculateE()}</div>
                                <div className="text-2xl font-bold text-purple-200">N/C</div>
                            </div>
                        </div>
                    </div>
                </motion.div>
            );
        };

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
                <motion.div initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} className="p-4 md:p-8">
                    <div className="flex items-center gap-3 mb-6">
                        <div className="p-3 bg-emerald-100 rounded-xl">
                            <Icon name="move-right" className="text-emerald-600" size={24} />
                        </div>
                        <div>
                            <h3 className="text-xl font-bold">ចលនាភាគល្អិតអគ្គិសនី (Particle Motion)</h3>
                            <p className="text-sm text-gray-500">គណនាសំទុះរបស់ភាគល្អិតក្នុងដែនអគ្គិសនី</p>
                        </div>
                    </div>
                    <div className="bg-emerald-50/50 p-4 rounded-xl border border-emerald-100 mb-8 flex items-start gap-3">
                        <Icon name="info" className="text-emerald-400 mt-0.5" size={18} />
                        <div className="text-sm text-emerald-800">
                            <span className="font-bold">រូបមន្ត៖</span> a = |q|E / m
                        </div>
                    </div>
                    <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
                        <div>
                            <SciInput label="បន្ទុកភាគល្អិត (q)" base={qBase} exp={qExp} onBaseChange={setQBase} onExpChange={setQExp} unit="C" colorClass="emerald" />
                            <SciInput label="ដែនអគ្គិសនី (E)" base={eBase} exp={eExp} onBaseChange={setEBase} onExpChange={setEExp} unit="N/C" colorClass="emerald" />
                            <SciInput label="ម៉ាសភាគល្អិត (m)" base={mBase} exp={mExp} onBaseChange={setMBase} onExpChange={setMExp} unit="kg" colorClass="emerald" />
                        </div>
                        <div className="flex flex-col h-full justify-center">
                            <div className="bg-gradient-to-br from-emerald-600 to-teal-700 rounded-3xl p-8 text-white shadow-xl text-center">
                                <div className="text-emerald-100 text-sm font-medium mb-4 uppercase">សំទុះរបស់ភាគល្អិត (a)</div>
                                <div className="text-4xl md:text-5xl font-black mb-2">{calculateA()}</div>
                                <div className="text-2xl font-bold text-emerald-200">m/s²</div>
                            </div>
                        </div>
                    </div>
                </motion.div>
            );
        };

        const TheoryTab = () => (
            <motion.div initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} className="p-4 md:p-8">
                <div className="flex items-center gap-3 mb-6">
                    <div className="p-3 bg-amber-100 rounded-xl">
                        <Icon name="book-open" className="text-amber-600" size={24} />
                    </div>
                    <div>
                        <h3 className="text-xl font-bold">ខ្លឹមសារសង្ខេបមេរៀន (Lesson Summary)</h3>
                        <p className="text-sm text-gray-500">មេរៀនទី១៖ បន្ទុក និងដែនអគ្គិសនី</p>
                    </div>
                </div>
                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div className="bg-slate-50 p-6 rounded-2xl border">
                        <h4 className="font-bold text-blue-700 flex items-center gap-2 mb-4"><Icon name="zap" size={18} /> ១. ច្បាប់គូឡុំ</h4>
                        <p className="text-sm text-gray-700 mb-4">កម្លាំងអន្តរកម្មរវាងពីរបន្ទុកចំណុចសមាមាត្រនឹងផលគុណនៃបន្ទុកទាំងពីរ និងច្រាសសមាមាត្រនឹងការរេនៃចម្ងាយ។</p>
                        <div className="bg-white p-3 rounded-xl border text-center text-blue-600 font-bold">F = k · |q₁ · q₂| / r²</div>
                    </div>
                    <div className="bg-slate-50 p-6 rounded-2xl border">
                        <h4 className="font-bold text-purple-700 flex items-center gap-2 mb-4"><Icon name="activity" size={18} /> ២. ដែនអគ្គិសនី</h4>
                        <p className="text-sm text-gray-700 mb-4">ជាលំហដែលព័ទ្ធជុំវិញបន្ទុកអគ្គិសនី ហើយផ្តល់នូវកម្លាំងអគ្គិសនីទៅលើបន្ទុកផ្សេងទៀត។</p>
                        <div className="bg-white p-3 rounded-xl border text-center text-purple-600 font-bold">E = k · |Q| / r²</div>
                    </div>
                    <div className="bg-slate-50 p-6 rounded-2xl border md:col-span-2">
                        <h4 className="font-bold text-emerald-700 flex items-center gap-2 mb-4"><Icon name="move-right" size={18} /> ៣. សំទុះភាគល្អិត</h4>
                        <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div className="bg-white p-3 rounded-xl border text-center text-emerald-600 font-bold">F = q · E</div>
                            <div className="bg-white p-3 rounded-xl border text-center text-emerald-600 font-bold">a = |q|E / m</div>
                        </div>
                    </div>
                </div>
            </motion.div>
        );

        const App = () => {
            const [activeTab, setActiveTab] = useState('theory');

            return (
                <div className="min-h-screen bg-slate-50 pb-12">
                    <header className="bg-slate-900 text-white pt-12 pb-24 relative overflow-hidden">
                        <div className="max-w-5xl mx-auto px-4 relative z-10 flex flex-col md:flex-row justify-between items-center gap-6">
                            <div className="flex items-center gap-5 text-center md:text-left">
                                <div className="w-16 h-16 bg-blue-600 rounded-2xl flex items-center justify-center shadow-xl">
                                    <Icon name="calculator" size={34} />
                                </div>
                                <div>
                                    <h1 className="text-3xl md:text-4xl font-bold font-moul">កម្មវិធីជំនួយការគណនារូបវិទ្យា</h1>
                                    <p className="text-blue-300 mt-1">ថ្នាក់ទី១១៖ បន្ទុក និងដែនអគ្គិសនី</p>
                                </div>
                            </div>
                            <div className="text-right hidden md:block">
                                <span className="text-slate-400 text-xs uppercase block">ផលិតដោយ</span>
                                <span className="text-xl font-bold">លោកគ្រូ ឡាង ចាន់ម៉ូលី</span>
                            </div>
                        </div>
                    </header>

                    <main className="max-w-5xl mx-auto px-4 -mt-12">
                        <div className="bg-white rounded-[2rem] shadow-2xl overflow-hidden border border-slate-100">
                            <nav className="flex p-2 bg-slate-50/50 border-b overflow-x-auto">
                                {[
                                    { id: 'theory', label: 'ខ្លឹមសារមេរៀន', icon: 'book-open' },
                                    { id: 'coulomb', label: 'ច្បាប់គូឡុំ', icon: 'zap' },
                                    { id: 'efield', label: 'ដែនអគ្គិសនី', icon: 'activity' },
                                    { id: 'motion', label: 'ចលនាភាគល្អិត', icon: 'move-right' },
                                ].map((tab) => (
                                    <button
                                        key={tab.id}
                                        onClick={() => setActiveTab(tab.id)}
                                        className={`flex-1 flex items-center justify-center gap-2 py-4 px-4 rounded-2xl font-bold transition-all whitespace-nowrap ${
                                            activeTab === tab.id ? 'bg-white shadow-sm ring-1 ring-slate-200 text-slate-900' : 'text-slate-500 hover:bg-slate-100'
                                        }`}
                                    >
                                        <Icon name={tab.icon} size={18} />
                                        <span className="inline">{tab.label}</span>
                                    </button>
                                ))}
                            </nav>

                            <div className="min-h-[400px]">
                                <AnimatePresence mode="wait">
                                    {activeTab === 'theory' && <TheoryTab key="t1" />}
                                    {activeTab === 'coulomb' && <CoulombTab key="c1" />}
                                    {activeTab === 'efield' && <EFieldTab key="e1" />}
                                    {activeTab === 'motion' && <MotionTab key="m1" />}
                                </AnimatePresence>
                            </div>

                            <footer className="bg-slate-50 border-t p-6 flex flex-col md:flex-row justify-between items-center gap-4">
                                <div className="flex items-center gap-2 text-slate-500 text-sm italic">
                                    <Icon name="book-open" size={16} />
                                    <span>រៀបចំសម្រាប់ជំនួយការបង្រៀនរបស់លោកគ្រូ ឡាង ចាន់ម៉ូលី</span>
                                </div>
                                <div className="px-4 py-1 bg-white rounded-full border text-xs font-bold text-slate-600">
                                    Version 1.0 (HTML)
                                </div>
                            </footer>
                        </div>
                    </main>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
