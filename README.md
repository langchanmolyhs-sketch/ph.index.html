<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>កម្មវិធីគណនារូបវិទ្យា</title>
    
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Kantumruy+Pro:wght@400;700&display=swap');
        body { font-family: 'Kantumruy Pro', sans-serif; }
    </style>
</head>
<body class="bg-gray-50">

    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        const PhysicApp = () => {
            const [q1, setQ1] = useState(2);
            const [q2, setQ2] = useState(3);
            const [r, setR] = useState(0.1);
            
            const k = 9e9;
            const force = (k * Math.abs(q1 * 1e-6 * q2 * 1e-6)) / Math.pow(r, 2);

            return (
                <div className="max-w-md mx-auto mt-10 p-6 bg-white rounded-2xl shadow-xl border border-blue-100">
                    <h1 className="text-2xl font-bold text-blue-700 mb-6 text-center">
                        ច្បាប់គូឡុំ (Coulomb's Law)
                    </h1>
                    
                    <div className="space-y-4">
                        <div>
                            <label className="block text-sm font-medium text-gray-700">បន្ទុក q1 (μC)</label>
                            <input type="number" value={q1} onChange={(e) => setQ1(e.target.value)} 
                                className="w-full p-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                        </div>
                        
                        <div>
                            <label className="block text-sm font-medium text-gray-700">បន្ទុក q2 (μC)</label>
                            <input type="number" value={q2} onChange={(e) => setQ2(e.target.value)} 
                                className="w-full p-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                        </div>
                        
                        <div>
                            <label className="block text-sm font-medium text-gray-700">ចម្ងាយ r (m)</label>
                            <input type="number" value={r} onChange={(e) => setR(e.target.value)} 
                                className="w-full p-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" />
                        </div>

                        <div className="mt-6 p-4 bg-blue-50 rounded-xl border border-blue-200">
                            <p className="text-sm text-blue-600 font-semibold text-center">កម្លាំងអន្តរកម្ម (F)</p>
                            <p className="text-3xl font-bold text-center text-blue-800">
                                {force.toExponential(3)} <span className="text-lg">N</span>
                            </p>
                        </div>
                    </div>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<PhysicApp />);
    </script>
</body>
</html>
