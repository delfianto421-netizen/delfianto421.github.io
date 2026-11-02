import React, { useState, useEffect, useRef } from 'react';
import { 
  BookOpen, 
  Feather, 
  Sparkles, 
  User, 
  Palette, 
  Layers, 
  FileText, 
  Download, 
  Copy, 
  Printer, 
  RefreshCw, 
  Plus, 
  Trash2, 
  Sliders, 
  Eye, 
  Check, 
  ChevronRight, 
  ChevronLeft, 
  Image as ImageIcon,
  Wand2,
  Clock,
  Type,
  Book,
  Maximize2,
  Share2,
  Settings,
  HelpCircle,
  FileCode
} from 'lucide-react';

// Sample Presets for Fast Testing
const SAMPLE_PRESETS = [
  {
    title: "Derai Hujan di Pelabuhan Sunda Kelapa",
    author: "Raden Mas Sasmita",
    theme: "Sastra Budaya & Realisme Magis",
    genre: "Drama Sejarah",
    targetWords: "2500 - 4000 Kata",
    characters: [
      { name: "Pak Sapardi", role: "Tokoh Utama", trait: "Penjaga mercusuar tua, pendiam, membawa kenangan perlawanan 1965", motivation: "Mencari kabar kebenaran tentang putri tunggalnya yang hilang" },
      { name: "Siti Rahayu", role: "Antagonis/Kunci", trait: "Wartawan muda ambisius dari ibu kota", motivation: "Membongkar rahasia surat-surat kuno yang tersembunyi" }
    ],
    setting: {
      place: "Dermaga kayu lapuk Pelabuhan Sunda Kelapa & Kedai Kopi Tua",
      time: "Senja temaram tahun 1988 saat badai laut selatan",
      atmosphere: "Melankolis, beraroma garam karsa, penuh bisikan misteri angin laut"
    },
    plotType: "Campuran (Maju & Flashback)",
    climax: "Petir menyambar tiang mercusuar tua; Rahayu menemukan peti tembaga berisi belasan surat yang ditulis dengan tinta darah mentah, sementara laut meluapkan ombak hitam yang menyerupai naga.",
    resolution: "Pak Sapardi menyerahkan kunci perunggu terakhir kepada Rahayu sebelum melangkah tenang menuju gulungan ombak, menghilang bersatu dengan pusaran air.",
    coda: "Bahwa setiap jejak sejarah tidak pernah sungguh hilang; ia hanya berpindah tempat menjadi riak gelombang yang terus berbisik pada siapapa pun yang sudi mendengar.",
    narrativePov: "Orang Ketiga Serba Tahu",
    rhetoricalStyle: ["Diksi Puitis & Arkais", "Metafora Sensorik Deep", "Personifikasi Alam"],
    paragraphGuides: [
      "Paragraf 1: Gambaran atmosfer senja di pelabuhan Sunda Kelapa saat kabut garam menyelimuti dermaga.",
      "Paragraf 2: Pengenalan sosok Pak Sapardi yang duduk meriung di atas peti kayu tua memegang lentera kusam.",
      "Paragraf 3: Kedatangan Siti Rahayu dengan derap langkah sepatu kulit yang memecah keheningan.",
      "Paragraf 4: Dialog tegang awal antara Rahayu dan Pak Sapardi mengenai arsip tahun 1965.",
      "Paragraf 5: Kilas balik (flashback) malam kelam ketika mercusuar pertama kali dipadamkan."
    ]
  },
  {
    title: "Simfoni Daun Kelabu di Lembah Merapi",
    author: "Dian Sastro Prawiro",
    theme: "Realisme Magis & Kemanusiaan",
    genre: "Fiksi Psikologis",
    targetWords: "1500 - 3000 Kata",
    characters: [
      { name: "Mbah Djojo", role: "Tokoh Utama", trait: "Empu penempa keris tua berjiwa tenang", motivation: "Menyelesaikan keris pusaka pembawa kedamaian desa" }
    ],
    setting: {
      place: "Lereng Gunung Merapi, Dusun Karangtengah",
      time: "Malam Jumat Kliwon musim kemarau panjang",
      atmosphere: "Mistis, hangat abu vulkanik, sunyi mencekam"
    },
    plotType: "Maju",
    climax: "Gemuruh Merapi mulai memuntahkan awan panas, Mbah Djojo menolak mengungsi demi mencelupkan keris terakhirnya ke cairan emas leburan.",
    resolution: "Suara gemuruh mendadak mereda seiring berkilauannya keris yang telah dingin di atas anvil penempaan.",
    coda: "Cinta manusia pada tanah kelahirannya sanggup mendinginkan amarah alam terbuas sekalipun.",
    narrativePov: "Orang Pertama ('Aku')",
    rhetoricalStyle: ["Majas Personifikasi", "Diksi Jawa Kuno & Puitis"],
    paragraphGuides: [
      "Paragraf 1: Aromanya abu dan belerang menyengat hidungku di ambang jendela.",
      "Paragraf 2: Mbah Djojo mengangkat palu besinya yang berdentang menghantam besi panas."
    ]
  }
];

export default function StoryStudioApp() {
  // Navigation State
  const [activeTab, setActiveTab] = useState('framework'); // framework | cover | reader
  const [currentStep, setCurrentStep] = useState(1);

  // Framework Form State
  const [formData, setFormData] = useState({
    title: "",
    author: "",
    theme: "Sastra Budaya & Realisme Magis",
    genre: "Drama Sastra",
    targetWords: "2000 - 3500 Kata",
    characters: [
      { name: "", role: "Tokoh Utama", trait: "", motivation: "" }
    ],
    setting: {
      place: "",
      time: "",
      atmosphere: ""
    },
    plotType: "Maju",
    climax: "",
    resolution: "",
    coda: "",
    narrativePov: "Orang Pertama ('Aku')",
    rhetoricalStyle: ["Diksi Puitis", "Metafora Deep"],
    paragraphGuides: ["Paragraf 1: Pembuka suasana...", "Paragraf 2: Pengenalan tokoh utama..."]
  });

  // Cover Style Customization State
  const [coverConfig, setCoverConfig] = useState({
    themePreset: 'minimalist-dark', // minimalist-dark | royal-gold | vintage-canvas | emerald-mystic | generative-ai
    fontFamily: 'serif', // serif | playfair | cinzel | sans
    accentColor: '#d97706',
    bgImageUrl: '',
    isGeneratingCoverArt: false,
    aiImagePrompt: ''
  });

  // Generation State
  const [isGeneratingStory, setIsGeneratingStory] = useState(false);
  const [generationProgress, setGenerationProgress] = useState(0);
  const [generationStatusText, setGenerationStatusText] = useState("");
  const [generatedStory, setGeneratedStory] = useState("");
  const [storyStats, setStoryStats] = useState({ words: 0, characters: 0, readTime: 0 });
  const [toastMessage, setToastMessage] = useState(null);

  // Reader Settings State
  const [readerFontSize, setReaderFontSize] = useState(18); // in px
  const [readerTheme, setReaderTheme] = useState('parchment'); // parchment | dark | clean

  // Notification Toast Helper
  const showToast = (msg) => {
    setToastMessage(msg);
    setTimeout(() => setToastMessage(null), 3500);
  };

  // Load Preset
  const handleApplyPreset = (preset) => {
    setFormData(JSON.parse(JSON.stringify(preset)));
    showToast(`Template "${preset.title}" berhasil dimuat!`);
  };

  // Character Handlers
  const handleAddCharacter = () => {
    setFormData(prev => ({
      ...prev,
      characters: [...prev.characters, { name: "", role: "Pendukung", trait: "", motivation: "" }]
    }));
  };

  const handleRemoveCharacter = (idx) => {
    setFormData(prev => ({
      ...prev,
      characters: prev.characters.filter((_, i) => i !== idx)
    }));
  };

  const handleCharacterChange = (idx, field, val) => {
    const updated = [...formData.characters];
    updated[idx][field] = val;
    setFormData(prev => ({ ...prev, characters: updated }));
  };

  // Paragraph Guide Handlers
  const handleAddParagraphGuide = () => {
    if (formData.paragraphGuides.length >= 80) {
      showToast("Maksimal 80 panduan paragraf telah tercapai.");
      return;
    }
    setFormData(prev => ({
      ...prev,
      paragraphGuides: [...prev.paragraphGuides, `Paragraf ${prev.paragraphGuides.length + 1}: `]
    }));
  };

  const handleRemoveParagraphGuide = (idx) => {
    setFormData(prev => ({
      ...prev,
      paragraphGuides: prev.paragraphGuides.filter((_, i) => i !== idx)
    }));
  };

  const handleParagraphGuideChange = (idx, val) => {
    const updated = [...formData.paragraphGuides];
    updated[idx] = val;
    setFormData(prev => ({ ...prev, paragraphGuides: updated }));
  };

  // Rhetorical Style Toggles
  const toggleRhetoricalStyle = (styleName) => {
    setFormData(prev => {
      const exists = prev.rhetoricalStyle.includes(styleName);
      if (exists) {
        return { ...prev, rhetoricalStyle: prev.rhetoricalStyle.filter(s => s !== styleName) };
      } else {
        return { ...prev, rhetoricalStyle: [...prev.rhetoricalStyle, styleName] };
      }
    });
  };

  // Calculate Story Stats
  useEffect(() => {
    if (!generatedStory) return;
    const cleanText = generatedStory.replace(/<[^>]*>?/gm, '');
    const words = cleanText.trim().split(/\s+/).filter(Boolean).length;
    const chars = cleanText.length;
    const readTime = Math.ceil(words / 200); // avg 200 wpm
    setStoryStats({ words, characters: chars, readTime });
  }, [generatedStory]);

  // Main Story Generation using Gemini API
  const handleGenerateStory = async () => {
    if (!formData.title || !formData.author) {
      showToast("Harap isi setidaknya Judul dan Nama Penulis!");
      setCurrentStep(1);
      return;
    }

    setIsGeneratingStory(true);
    setGenerationProgress(15);
    setGenerationStatusText("Merajut kerangka & mengolah diksi puitis sastra...");

    // Build Master Prompt for Gemini
    const systemPrompt = `Anda adalah seorang Maestro Penulis Cerpen Sastra Indonesia terkemuka (setara kelas Pramoedya Ananta Toer, Seno Gumira Ajidarma, dan Eka Kurniawan).
Tugas Anda adalah mengarang sebuah Cerita Pendek (Cerpen) utuh bernilai sastra tinggi, kaya gaya bahasa, alur emosional mendalam, detail deskripsi sensorik (penglihatan, pendengaran, penciuman, perabaan), serta diksi yang sangat kaya sesuai tema.

ATURAN PENTING GENERASI CERPEN:
1. Cerpen HARUS ditulis secara utuh dari awal sampai koda/penutup.
2. Panjang cerita harus sangat kaya dan panjang (menuju target ${formData.targetWords}), tidak boleh terpotong atau dipersingkat.
3. Gunakan paragraf yang mengalir indah, majas retoris yang elegan, dan dialog yang berbobot.
4. Jangan menyertakan komentar meta seperti "Berikut adalah cerpennya", langsung mulai dengan Judul dan teks naskah cerpen sastra.
5. Gunakan format Markdown yang rapi (Judul #, Subjudul/Bagian ## jika ada, dan paragraf-paragraf yang tertata).`;

    const userQuery = `
JUDUL CERPEN: "${formData.title}"
NAMA PENULIS: ${formData.author}
TEMA CERITA: ${formData.theme}
GENRE: ${formData.genre}
TARGET PANJANG: ${formData.targetWords}

SUDUT PANDANG NARASI: ${formData.narrativePov}
GAYA BAHASA & MAJAS: ${formData.rhetoricalStyle.join(', ')}

KERANGKA TOKOH & KARAKTER:
${formData.characters.map((c, i) => `${i + 1}. ${c.name} (${c.role}): Sifat [${c.trait}], Motivasi [${c.motivation}]`).join('\n')}

LATAR CERITA (SETTING):
- Tempat: ${formData.setting.place || 'Tidak ditentukan secara spesifik'}
- Waktu: ${formData.setting.time || 'Masa sekarang'}
- Suasana/Atmosfer: ${formData.setting.atmosphere || 'Melankolis & Penuh makna'}

STRUKTUR ALUR / PLOT: ${formData.plotType}
- Puncak Konflik (Klimaks): ${formData.climax || 'Munculnya krisis utama yang merubah arah hidup tokoh utama'}
- Resolusi: ${formData.resolution || 'Penyelesaian yang berkesan dan emosional'}
- Koda / Penutup: ${formData.coda || 'Pesan tersirat yang membendung di benak pembaca'}

PANDUAN ALUR PARAGRAF (IDE UTAMA PER PARAGRAF):
${formData.paragraphGuides.map((g, i) => `${g}`).join('\n')}

Silakan karang cerpen masterpice ini sekarang dengan bahasa Indonesia sastra yang amat indah!
`;

    try {
      setGenerationProgress(45);
      setGenerationStatusText("Merangkai konflik & letupan emosi antar tokoh...");

      const apiKey = ""; // Canvas runtime automatically handles this empty key
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

      const payload = {
        contents: [{ parts: [{ text: userQuery }] }],
        systemInstruction: { parts: [{ text: systemPrompt }] }
      };

      const res = await fetch(apiUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      setGenerationProgress(80);
      setGenerationStatusText("Menyempurnakan koda & memoles keindahan bahasa...");

      const data = await res.json();
      const storyText = data?.candidates?.[0]?.content?.parts?.[0]?.text;

      if (storyText) {
        setGeneratedStory(storyText);
        setGenerationProgress(100);
        setTimeout(() => {
          setIsGeneratingStory(false);
          setActiveTab('reader');
          showToast("Cerpen sastra berhasil dikarang!");
        }, 500);
      } else {
        throw new Error("Respon dari AI tidak menyertakan teks.");
      }
    } catch (err) {
      console.error("Story Generation Error:", err);
      setIsGeneratingStory(false);
      showToast("Gagal mengarang cerpen. Silakan coba lagi atau periksa koneksi internet.");
    }
  };

  // Cover Image Generation using Gemini 3.1 Flash Image
  const handleGenerateAICoverArt = async () => {
    setCoverConfig(prev => ({ ...prev, isGeneratingCoverArt: true }));
    showToast("Menggambar lukisan sampul sastra dengan AI...");

    const imagePrompt = `An artistic high-end book cover artwork for a short story titled "${formData.title}". 
Theme: ${formData.theme}. Mood: ${formData.setting.atmosphere || 'literary atmosphere'}. 
Style: Fine art painting, mysterious, atmospheric, detailed oil on canvas aesthetic, elegant color composition, masterpiece, no text overlay.`;

    try {
      const apiKey = "";
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image:generateContent?key=${apiKey}`;

      const payload = {
        contents: [{ parts: [{ text: imagePrompt }] }],
        generationConfig: {
          responseModalities: ['IMAGE'],
          imageConfig: { aspectRatio: "3:4" }
        }
      };

      const res = await fetch(apiUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      const result = await res.json();
      const base64Data = result?.candidates?.[0]?.content?.parts?.find(p => p.inlineData)?.inlineData?.data;

      if (base64Data) {
        setCoverConfig(prev => ({
          ...prev,
          bgImageUrl: `data:image/png;base64,${base64Data}`,
          themePreset: 'generative-ai',
          isGeneratingCoverArt: false
        }));
        showToast("Lukisan sampul AI berhasil diciptakan!");
      } else {
        throw new Error("Gagal menerima data gambar.");
      }
    } catch (err) {
      console.error("Cover Art Gen Error:", err);
      setCoverConfig(prev => ({ ...prev, isGeneratingCoverArt: false }));
      showToast("Gagal membuat ilustrasi sampul. Menggunakan latar Ornamen Sastra.");
    }
  };

  // Export & Copy Helpers
  const handleCopyStory = () => {
    if (!generatedStory) return;
    const cleanText = generatedStory.replace(/<[^>]*>?/gm, '');
    navigator.clipboard.writeText(`JUDUL: ${formData.title}\nKARYA: ${formData.author}\n\n${cleanText}`);
    showToast("Naskah cerpen berhasil disalin ke clipboard!");
  };

  const handleDownloadTXT = () => {
    if (!generatedStory) return;
    const element = document.createElement("a");
    const cleanText = generatedStory.replace(/<[^>]*>?/gm, '');
    const file = new Blob([`JUDUL: ${formData.title.toUpperCase()}\nNAMA PENULIS: ${formData.author}\nTEMA: ${formData.theme}\n${'='.repeat(40)}\n\n${cleanText}`], { type: 'text/plain;charset=utf-8' });
    element.href = URL.createObjectURL(file);
    element.download = `${formData.title.replace(/\s+/g, '_')}_Cerpen_Sastra.txt`;
    document.body.appendChild(element);
    element.click();
    document.body.removeChild(element);
    showToast("File TXT berhasil diunduh.");
  };

  const handlePrint = () => {
    window.print();
  };

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 flex flex-col font-sans antialiased selection:bg-amber-500 selection:text-slate-950">
      
      {/* Toast Notification */}
      {toastMessage && (
        <div className="fixed bottom-6 right-6 z-50 bg-amber-500 text-slate-950 px-5 py-3 rounded-xl shadow-2xl font-medium flex items-center gap-3 animate-bounce">
          <Sparkles className="w-5 h-5 flex-shrink-0" />
          <span>{toastMessage}</span>
        </div>
      )}

      {/* Application Top Bar Header */}
      <header className="sticky top-0 z-40 bg-slate-900/90 backdrop-blur-md border-b border-slate-800/80 px-4 lg:px-8 py-3.5 flex items-center justify-between">
        <div className="flex items-center gap-3">
          <div className="w-10 h-10 rounded-xl bg-gradient-to-tr from-amber-600 via-amber-500 to-amber-300 flex items-center justify-center shadow-lg shadow-amber-500/20">
            <Feather className="w-5 h-5 text-slate-950" />
          </div>
          <div>
            <h1 className="font-serif text-lg lg:text-xl font-bold tracking-tight text-slate-50 flex items-center gap-2">
              Aplikasi Penulis Cerpen Sastra
              <span className="text-[10px] font-sans font-semibold tracking-wider px-2 py-0.5 rounded-full bg-amber-500/10 text-amber-400 border border-amber-500/30 uppercase">
                Studio Maestro AI
              </span>
            </h1>
            <p className="text-xs text-slate-400 hidden sm:block">Perancang Kerangka Cerita & Platform Generasi Karya Sastra Indonesia</p>
          </div>
        </div>

        {/* Quick Presets & Navigation Tabs */}
        <div className="flex items-center gap-2">
          {/* Preset Selector Dropdown */}
          <div className="relative group">
            <button className="px-3 py-2 text-xs font-medium rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700/70 flex items-center gap-2 transition-all">
              <Sparkles className="w-3.5 h-3.5 text-amber-400" />
              <span className="hidden md:inline">Contoh Template Sastra</span>
            </button>
            <div className="absolute right-0 mt-2 w-64 bg-slate-900 border border-slate-800 rounded-xl shadow-2xl p-2 hidden group-hover:block z-50">
              <div className="text-[11px] font-semibold text-slate-400 px-3 py-1 uppercase tracking-wider">Pilih Template</div>
              {SAMPLE_PRESETS.map((p, idx) => (
                <button
                  key={idx}
                  onClick={() => handleApplyPreset(p)}
                  className="w-full text-left p-2.5 rounded-lg hover:bg-slate-800 transition-all text-xs"
                >
                  <div className="font-serif font-bold text-slate-200">{p.title}</div>
                  <div className="text-[11px] text-slate-400 mt-0.5">{p.genre} • {p.author}</div>
                </button>
              ))}
            </div>
          </div>

          {/* View Switcher Tabs */}
          <div className="bg-slate-950 p-1 rounded-xl border border-slate-800/80 flex items-center gap-1">
            <button
              onClick={() => setActiveTab('framework')}
              className={`px-3.5 py-1.5 rounded-lg text-xs font-medium transition-all flex items-center gap-1.5 ${
                activeTab === 'framework' 
                  ? 'bg-amber-500 text-slate-950 font-semibold shadow-md' 
                  : 'text-slate-400 hover:text-slate-200 hover:bg-slate-900'
              }`}
            >
              <Sliders className="w-3.5 h-3.5" />
              <span>1. Kerangka Cerita</span>
            </button>
            <button
              onClick={() => setActiveTab('cover')}
              className={`px-3.5 py-1.5 rounded-lg text-xs font-medium transition-all flex items-center gap-1.5 ${
                activeTab === 'cover' 
                  ? 'bg-amber-500 text-slate-950 font-semibold shadow-md' 
                  : 'text-slate-400 hover:text-slate-200 hover:bg-slate-900'
              }`}
            >
              <Book className="w-3.5 h-3.5" />
              <span>2. Sampul Buku</span>
            </button>
            <button
              onClick={() => setActiveTab('reader')}
              className={`px-3.5 py-1.5 rounded-lg text-xs font-medium transition-all flex items-center gap-1.5 ${
                activeTab === 'reader' 
                  ? 'bg-amber-500 text-slate-950 font-semibold shadow-md' 
                  : 'text-slate-400 hover:text-slate-200 hover:bg-slate-900'
              }`}
            >
              <BookOpen className="w-3.5 h-3.5" />
              <span>3. Studio Baca Cerpen</span>
            </button>
          </div>
        </div>
      </header>

      {/* Main Studio Body Area */}
      <main className="flex-1 flex flex-col max-w-7xl w-full mx-auto p-4 lg:p-6 gap-6">

        {/* ==================================================================== */}
        {/* TAB 1: FRAMEWORK & STORY BUILDER WIZARD                              */}
        {/* ==================================================================== */}
        {activeTab === 'framework' && (
          <div className="grid grid-cols-1 lg:grid-cols-12 gap-6">

            {/* Left Column: Form Stepper Steps */}
            <div className="lg:col-span-8 flex flex-col gap-6">
              
              {/* Stepper Navigation Pills */}
              <div className="bg-slate-900/80 border border-slate-800 rounded-2xl p-3 flex items-center justify-between gap-2 overflow-x-auto">
                {[
                  { id: 1, label: "Identitas & Tema", icon: Book },
                  { id: 2, label: "Tokoh & Latar", icon: User },
                  { id: 3, label: "Alur & Konflik", icon: Layers },
                  { id: 4, label: "Gaya & Paragraf", icon: Feather },
                ].map((s) => {
                  const IconComp = s.icon;
                  const isActive = currentStep === s.id;
                  return (
                    <button
                      key={s.id}
                      onClick={() => setCurrentStep(s.id)}
                      className={`flex-1 min-w-[130px] px-3 py-2.5 rounded-xl text-xs font-medium flex items-center justify-center gap-2 transition-all ${
                        isActive
                          ? 'bg-gradient-to-r from-amber-600 to-amber-500 text-slate-950 font-bold shadow-lg shadow-amber-500/10'
                          : 'bg-slate-950/50 text-slate-400 hover:bg-slate-800 hover:text-slate-200 border border-slate-800/60'
                      }`}
                    >
                      <IconComp className="w-4 h-4 flex-shrink-0" />
                      <span className="truncate">{s.label}</span>
                    </button>
                  );
                })}
              </div>

              {/* STEP 1: IDENTITAS & TEMA */}
              {currentStep === 1 && (
                <div className="bg-slate-900/60 border border-slate-800 rounded-2xl p-6 flex flex-col gap-5 shadow-xl">
                  <div className="flex items-center justify-between border-b border-slate-800/80 pb-4">
                    <div>
                      <h2 className="text-lg font-serif font-bold text-amber-400 flex items-center gap-2">
                        <Book className="w-5 h-5 text-amber-500" /> Identitas Cerpen & Tema Sastra
                      </h2>
                      <p className="text-xs text-slate-400 mt-0.5">Tentukan judul karya, kredensial penulis, serta orientasi genre cerita.</p>
                    </div>
                    <span className="text-xs text-slate-500 font-mono">Langkah 1 dari 4</span>
                  </div>

                  <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                      <label className="block text-xs font-medium text-slate-300 mb-1.5">Judul Cerita Pendek <span className="text-amber-500">*</span></label>
                      <input
                        type="text"
                        placeholder="Contoh: Senandung Sepi di Ujung Senja"
                        value={formData.title}
                        onChange={(e) => setFormData({ ...formData, title: e.target.value })}
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-slate-100 focus:outline-none focus:border-amber-500 transition-colors"
                      />
                    </div>

                    <div>
                      <label className="block text-xs font-medium text-slate-300 mb-1.5">Nama Penulis / Pengarang <span className="text-amber-500">*</span></label>
                      <input
                        type="text"
                        placeholder="Contoh: Raden Mas Sasmita"
                        value={formData.author}
                        onChange={(e) => setFormData({ ...formData, author: e.target.value })}
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-slate-100 focus:outline-none focus:border-amber-500 transition-colors"
                      />
                    </div>
                  </div>

                  <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                      <label className="block text-xs font-medium text-slate-300 mb-1.5">Tema Utama</label>
                      <select
                        value={formData.theme}
                        onChange={(e) => setFormData({ ...formData, theme: e.target.value })}
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2.5 text-xs text-slate-200 focus:outline-none focus:border-amber-500"
                      >
                        <option>Sastra Budaya & Realisme Magis</option>
                        <option>Cinta Klasik & Melankolia</option>
                        <option>Misteri Psikologis & Eksistensial</option>
                        <option>Kemanusiaan & Kritik Sosial</option>
                        <option>Distopia & Sci-Fi Lokal</option>
                        <option>Filsafat & Spiritualisme</option>
                      </select>
                    </div>

                    <div>
                      <label className="block text-xs font-medium text-slate-300 mb-1.5">Genre Cerita</label>
                      <input
                        type="text"
                        placeholder="Drama Sastra, Thriller, Fabel..."
                        value={formData.genre}
                        onChange={(e) => setFormData({ ...formData, genre: e.target.value })}
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2.5 text-xs text-slate-100 focus:outline-none focus:border-amber-500"
                      />
                    </div>

                    <div>
                      <label className="block text-xs font-medium text-slate-300 mb-1.5">Target Panjang Kata</label>
                      <select
                        value={formData.targetWords}
                        onChange={(e) => setFormData({ ...formData, targetWords: e.target.value })}
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2.5 text-xs text-slate-200 focus:outline-none focus:border-amber-500"
                      >
                        <option>1000 - 2000 Kata (Ringkas)</option>
                        <option>2000 - 3500 Kata (Standar Sastra)</option>
                        <option>3500 - 6000 Kata (Mendalam)</option>
                        <option>6000 - 10000 Kata (Novelet Sastra)</option>
                      </select>
                    </div>
                  </div>

                  {/* Navigation Buttons */}
                  <div className="flex justify-end pt-4 border-t border-slate-800/80">
                    <button
                      onClick={() => setCurrentStep(2)}
                      className="bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold px-5 py-2.5 rounded-xl text-xs flex items-center gap-2 transition-all shadow-lg shadow-amber-500/20"
                    >
                      <span>Lanjut: Tokoh & Latar</span>
                      <ChevronRight className="w-4 h-4" />
                    </button>
                  </div>
                </div>
              )}

              {/* STEP 2: TOKOH & LATAR */}
              {currentStep === 2 && (
                <div className="bg-slate-900/60 border border-slate-800 rounded-2xl p-6 flex flex-col gap-5 shadow-xl">
                  <div className="flex items-center justify-between border-b border-slate-800/80 pb-4">
                    <div>
                      <h2 className="text-lg font-serif font-bold text-amber-400 flex items-center gap-2">
                        <User className="w-5 h-5 text-amber-500" /> Karakterisasi Tokoh & Latar Cerita
                      </h2>
                      <p className="text-xs text-slate-400 mt-0.5">Rancang watak tokoh, motivasi internal, serta atmosfer ruang dan waktu.</p>
                    </div>
                    <span className="text-xs text-slate-500 font-mono">Langkah 2 dari 4</span>
                  </div>

                  {/* Character List Builder */}
                  <div className="flex flex-col gap-3">
                    <div className="flex items-center justify-between">
                      <label className="text-xs font-semibold uppercase tracking-wider text-slate-300">Daftar Tokoh Cerita</label>
                      <button
                        onClick={handleAddCharacter}
                        className="text-xs text-amber-400 hover:text-amber-300 font-medium flex items-center gap-1 bg-amber-500/10 px-3 py-1 rounded-lg border border-amber-500/20"
                      >
                        <Plus className="w-3.5 h-3.5" /> Tambah Tokoh
                      </button>
                    </div>

                    {formData.characters.map((c, idx) => (
                      <div key={idx} className="bg-slate-950/80 border border-slate-800 rounded-xl p-4 flex flex-col gap-3 relative group">
                        <div className="flex items-center justify-between">
                          <span className="text-xs font-bold text-amber-500">Tokoh #{idx + 1}</span>
                          {formData.characters.length > 1 && (
                            <button
                              onClick={() => handleRemoveCharacter(idx)}
                              className="text-red-400 hover:text-red-300 p-1"
                              title="Hapus Tokoh"
                            >
                              <Trash2 className="w-3.5 h-3.5" />
                            </button>
                          )}
                        </div>
                        <div className="grid grid-cols-1 md:grid-cols-3 gap-3">
                          <div>
                            <input
                              type="text"
                              placeholder="Nama Tokoh"
                              value={c.name}
                              onChange={(e) => handleCharacterChange(idx, 'name', e.target.value)}
                              className="w-full bg-slate-900 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-100"
                            />
                          </div>
                          <div>
                            <select
                              value={c.role}
                              onChange={(e) => handleCharacterChange(idx, 'role', e.target.value)}
                              className="w-full bg-slate-900 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200"
                            >
                              <option>Tokoh Utama (Protagonis)</option>
                              <option>Antagonis / Kunci</option>
                              <option>Pendukung Utama</option>
                              <option>Tokoh Figuratif / Pembantu</option>
                            </select>
                          </div>
                          <div>
                            <input
                              type="text"
                              placeholder="Sifat / Karakter (Misal: Melankolis, Ambisius)"
                              value={c.trait}
                              onChange={(e) => handleCharacterChange(idx, 'trait', e.target.value)}
                              className="w-full bg-slate-900 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-100"
                            />
                          </div>
                        </div>
                        <div>
                          <input
                            type="text"
                            placeholder="Motivasi / Keinginan Tersembunyi Tokoh..."
                            value={c.motivation}
                            onChange={(e) => handleCharacterChange(idx, 'motivation', e.target.value)}
                            className="w-full bg-slate-900 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-100"
                          />
                        </div>
                      </div>
                    ))}
                  </div>

                  {/* Setting Inputs */}
                  <div className="border-t border-slate-800/80 pt-4 flex flex-col gap-3">
                    <label className="text-xs font-semibold uppercase tracking-wider text-slate-300">Latar Cerita (Setting)</label>
                    <div className="grid grid-cols-1 md:grid-cols-3 gap-3">
                      <div>
                        <label className="block text-[11px] text-slate-400 mb-1">Latar Tempat</label>
                        <input
                          type="text"
                          placeholder="Dermaga kayu, Rumah tua, Hutan rimba..."
                          value={formData.setting.place}
                          onChange={(e) => setFormData({ ...formData, setting: { ...formData.setting, place: e.target.value } })}
                          className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs text-slate-100"
                        />
                      </div>
                      <div>
                        <label className="block text-[11px] text-slate-400 mb-1">Latar Waktu</label>
                        <input
                          type="text"
                          placeholder="Malam Jumat Kliwon 1998, Senja..."
                          value={formData.setting.time}
                          onChange={(e) => setFormData({ ...formData, setting: { ...formData.setting, time: e.target.value } })}
                          className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs text-slate-100"
                        />
                      </div>
                      <div>
                        <label className="block text-[11px] text-slate-400 mb-1">Suasana & Atmosfer</label>
                        <input
                          type="text"
                          placeholder="Garam karsa, Mencekam, Sunyi puitis..."
                          value={formData.setting.atmosphere}
                          onChange={(e) => setFormData({ ...formData, setting: { ...formData.setting, atmosphere: e.target.value } })}
                          className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs text-slate-100"
                        />
                      </div>
                    </div>
                  </div>

                  {/* Navigation */}
                  <div className="flex justify-between pt-4 border-t border-slate-800/80">
                    <button
                      onClick={() => setCurrentStep(1)}
                      className="bg-slate-800 hover:bg-slate-700 text-slate-300 px-4 py-2.5 rounded-xl text-xs flex items-center gap-1.5"
                    >
                      <ChevronLeft className="w-4 h-4" /> Kembali
                    </button>
                    <button
                      onClick={() => setCurrentStep(3)}
                      className="bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold px-5 py-2.5 rounded-xl text-xs flex items-center gap-2 transition-all shadow-lg shadow-amber-500/20"
                    >
                      <span>Lanjut: Alur & Konflik</span>
                      <ChevronRight className="w-4 h-4" />
                    </button>
                  </div>
                </div>
              )}

              {/* STEP 3: ALUR & PUNCAK KONFLIK */}
              {currentStep === 3 && (
                <div className="bg-slate-900/60 border border-slate-800 rounded-2xl p-6 flex flex-col gap-5 shadow-xl">
                  <div className="flex items-center justify-between border-b border-slate-800/80 pb-4">
                    <div>
                      <h2 className="text-lg font-serif font-bold text-amber-400 flex items-center gap-2">
                        <Layers className="w-5 h-5 text-amber-500" /> Struktur Alur, Klimaks & Koda
                      </h2>
                      <p className="text-xs text-slate-400 mt-0.5">Susun lintasan alur narasi dari krisis puncak hingga pesan filosofis penutup.</p>
                    </div>
                    <span className="text-xs text-slate-500 font-mono">Langkah 3 dari 4</span>
                  </div>

                  <div>
                    <label className="block text-xs font-medium text-slate-300 mb-1.5">Jenis Alur (Plot)</label>
                    <select
                      value={formData.plotType}
                      onChange={(e) => setFormData({ ...formData, plotType: e.target.value })}
                      className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-xs text-slate-200"
                    >
                      <option>Maju (Linear)</option>
                      <option>Mundur / Sorot Balik (Flashback)</option>
                      <option>Campuran (Maju-Mundur Bergantian)</option>
                      <option>In Medias Res (Langsung di Tengah Konflik)</option>
                    </select>
                  </div>

                  <div>
                    <label className="block text-xs font-medium text-slate-300 mb-1.5">
                      Puncak Konflik (Klimaks Cerita) <span className="text-amber-500">*</span>
                    </label>
                    <textarea
                      rows={3}
                      placeholder="Gambarkan titik puncak ketegangan, benturan keinginan antar tokoh, atau rahasia utama yang akhirnya terbongkar..."
                      value={formData.climax}
                      onChange={(e) => setFormData({ ...formData, climax: e.target.value })}
                      className="w-full bg-slate-950 border border-slate-800 rounded-xl p-3 text-xs text-slate-100 focus:outline-none focus:border-amber-500"
                    />
                  </div>

                  <div>
                    <label className="block text-xs font-medium text-slate-300 mb-1.5">Resolusi (Peleraian)</label>
                    <textarea
                      rows={2}
                      placeholder="Bagaimana konflik mereda dan apa dampak keputusannya terhadap tokoh..."
                      value={formData.resolution}
                      onChange={(e) => setFormData({ ...formData, resolution: e.target.value })}
                      className="w-full bg-slate-950 border border-slate-800 rounded-xl p-3 text-xs text-slate-100 focus:outline-none focus:border-amber-500"
                    />
                  </div>

                  <div>
                    <label className="block text-xs font-medium text-slate-300 mb-1.5">Koda / Pesan Amanat Penutup</label>
                    <textarea
                      rows={2}
                      placeholder="Renungan filosofis akhir yang membekas di hati pembaca..."
                      value={formData.coda}
                      onChange={(e) => setFormData({ ...formData, coda: e.target.value })}
                      className="w-full bg-slate-950 border border-slate-800 rounded-xl p-3 text-xs text-slate-100 focus:outline-none focus:border-amber-500"
                    />
                  </div>

                  {/* Navigation */}
                  <div className="flex justify-between pt-4 border-t border-slate-800/80">
                    <button
                      onClick={() => setCurrentStep(2)}
                      className="bg-slate-800 hover:bg-slate-700 text-slate-300 px-4 py-2.5 rounded-xl text-xs flex items-center gap-1.5"
                    >
                      <ChevronLeft className="w-4 h-4" /> Kembali
                    </button>
                    <button
                      onClick={() => setCurrentStep(4)}
                      className="bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold px-5 py-2.5 rounded-xl text-xs flex items-center gap-2 transition-all shadow-lg shadow-amber-500/20"
                    >
                      <span>Lanjut: Gaya Bahasa & Paragraf</span>
                      <ChevronRight className="w-4 h-4" />
                    </button>
                  </div>
                </div>
              )}

              {/* STEP 4: GAYA BAHASA & PARAGRAF (1 - 80) */}
              {currentStep === 4 && (
                <div className="bg-slate-900/60 border border-slate-800 rounded-2xl p-6 flex flex-col gap-5 shadow-xl">
                  <div className="flex items-center justify-between border-b border-slate-800/80 pb-4">
                    <div>
                      <h2 className="text-lg font-serif font-bold text-amber-400 flex items-center gap-2">
                        <Feather className="w-5 h-5 text-amber-500" /> Gaya Retorika & Panduan 1-80 Paragraf
                      </h2>
                      <p className="text-xs text-slate-400 mt-0.5">Atur warna sudut pandang, gaya majas sastra, serta ide gagasan tiap paragraf.</p>
                    </div>
                    <span className="text-xs text-slate-500 font-mono">Langkah 4 dari 4</span>
                  </div>

                  <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                      <label className="block text-xs font-medium text-slate-300 mb-1.5">Sudut Pandang Narasi (POV)</label>
                      <select
                        value={formData.narrativePov}
                        onChange={(e) => setFormData({ ...formData, narrativePov: e.target.value })}
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2.5 text-xs text-slate-200"
                      >
                        <option>Orang Pertama ('Aku' / Subjektif Intim)</option>
                        <option>Orang Ketiga Serba Tahu ('Dia' / Serbapaham)</option>
                        <option>Orang Ketiga Terbatas ('Dia' / Dari Sudut Tokoh Utama)</option>
                        <option>Orang Kedua ('Kamu' / Eksperimental)</option>
                      </select>
                    </div>

                    <div>
                      <label className="block text-xs font-medium text-slate-300 mb-1.5">Pilihan Style Retorika & Diksi</label>
                      <div className="flex flex-wrap gap-1.5">
                        {[
                          "Diksi Puitis & Arkais",
                          "Metafora Sensorik Deep",
                          "Personifikasi Alam",
                          "Dialog Filosofis",
                          "Depresi & Melankolia",
                          "Sarkastik & Tajam",
                          "Realistis & Lugas"
                        ].map((st, i) => {
                          const active = formData.rhetoricalStyle.includes(st);
                          return (
                            <button
                              key={i}
                              type="button"
                              onClick={() => toggleRhetoricalStyle(st)}
                              className={`px-2.5 py-1 rounded-lg text-[11px] font-medium border transition-all ${
                                active
                                  ? 'bg-amber-500/20 text-amber-300 border-amber-500/50'
                                  : 'bg-slate-950 text-slate-400 border-slate-800 hover:text-slate-200'
                              }`}
                            >
                              {st}
                            </button>
                          );
                        })}
                      </div>
                    </div>
                  </div>

                  {/* Dynamic 1-80 Paragraph Guide Builder */}
                  <div className="border-t border-slate-800/80 pt-4 flex flex-col gap-3">
                    <div className="flex items-center justify-between">
                      <div>
                        <label className="text-xs font-semibold uppercase tracking-wider text-slate-300 flex items-center gap-2">
                          Modul Kalimat Utama Paragraf ({formData.paragraphGuides.length} dari 80 Paragraf)
                        </label>
                        <p className="text-[11px] text-slate-400">Tentukan ide pokok/kalimat awal yang wajib ada di setiap paragraf.</p>
                      </div>
                      <button
                        onClick={handleAddParagraphGuide}
                        className="text-xs text-amber-400 hover:text-amber-300 font-medium flex items-center gap-1 bg-amber-500/10 px-3 py-1 rounded-lg border border-amber-500/20"
                      >
                        <Plus className="w-3.5 h-3.5" /> Tambah Paragraf
                      </button>
                    </div>

                    <div className="max-h-[300px] overflow-y-auto pr-1 flex flex-col gap-2">
                      {formData.paragraphGuides.map((guide, idx) => (
                        <div key={idx} className="flex items-center gap-2">
                          <span className="text-[11px] font-mono text-slate-500 w-8 text-right flex-shrink-0">#{idx + 1}</span>
                          <input
                            type="text"
                            value={guide}
                            onChange={(e) => handleParagraphGuideChange(idx, e.target.value)}
                            placeholder={`Gagasan paragraf ke-${idx + 1}...`}
                            className="flex-1 bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-xs text-slate-200 focus:outline-none focus:border-amber-500"
                          />
                          {formData.paragraphGuides.length > 1 && (
                            <button
                              onClick={() => handleRemoveParagraphGuide(idx)}
                              className="text-slate-500 hover:text-red-400 p-1"
                            >
                              <Trash2 className="w-3.5 h-3.5" />
                            </button>
                          )}
                        </div>
                      ))}
                    </div>
                  </div>

                  {/* Navigation & Big Generate Button */}
                  <div className="flex justify-between items-center pt-4 border-t border-slate-800/80">
                    <button
                      onClick={() => setCurrentStep(3)}
                      className="bg-slate-800 hover:bg-slate-700 text-slate-300 px-4 py-2.5 rounded-xl text-xs flex items-center gap-1.5"
                    >
                      <ChevronLeft className="w-4 h-4" /> Kembali
                    </button>

                    <button
                      onClick={handleGenerateStory}
                      disabled={isGeneratingStory}
                      className="bg-gradient-to-r from-amber-500 via-amber-400 to-amber-500 hover:from-amber-400 hover:to-amber-300 text-slate-950 font-bold px-7 py-3 rounded-xl text-sm flex items-center gap-2.5 transition-all shadow-xl shadow-amber-500/20 active:scale-95 disabled:opacity-50"
                    >
                      <Wand2 className="w-4 h-4" />
                      <span>{isGeneratingStory ? 'Mengarang Karya Sastra...' : 'GENERATE CERPEN SEKARANG'}</span>
                    </button>
                  </div>
                </div>
              )}

            </div>

            {/* Right Column: Dynamic Live Overview Card */}
            <div className="lg:col-span-4 flex flex-col gap-6">
              {/* Story Summary Card */}
              <div className="bg-slate-900/80 border border-slate-800 rounded-2xl p-5 flex flex-col gap-4 shadow-xl sticky top-20">
                <div className="border-b border-slate-800 pb-3 flex items-center justify-between">
                  <h3 className="font-serif font-bold text-slate-200 text-sm flex items-center gap-2">
                    <FileText className="w-4 h-4 text-amber-500" /> Ringkasan Kerangka Cerita
                  </h3>
                  <span className="text-[10px] bg-slate-800 text-amber-400 px-2 py-0.5 rounded font-mono">Pratinjau</span>
                </div>

                <div className="space-y-3 text-xs">
                  <div>
                    <span className="text-slate-400 block text-[11px]">Judul & Penulis</span>
                    <p className="font-serif font-bold text-amber-400 text-base">{formData.title || '(Belum ada judul)'}</p>
                    <p className="text-slate-300 font-medium">Karya: {formData.author || '(Nama Penulis)'}</p>
                  </div>

                  <div className="grid grid-cols-2 gap-2 pt-2 border-t border-slate-800/60">
                    <div>
                      <span className="text-slate-500 block text-[10px]">Tema</span>
                      <span className="text-slate-300 font-medium truncate block">{formData.theme}</span>
                    </div>
                    <div>
                      <span className="text-slate-500 block text-[10px]">Target Kata</span>
                      <span className="text-slate-300 font-medium block">{formData.targetWords}</span>
                    </div>
                  </div>

                  <div className="pt-2 border-t border-slate-800/60">
                    <span className="text-slate-500 block text-[10px]">Daftar Tokoh ({formData.characters.length})</span>
                    <div className="flex flex-wrap gap-1 mt-1">
                      {formData.characters.map((c, i) => (
                        <span key={i} className="bg-slate-950 text-slate-300 px-2 py-0.5 rounded text-[10px] border border-slate-800">
                          {c.name || `Tokoh ${i+1}`} ({c.role})
                        </span>
                      ))}
                    </div>
                  </div>

                  <div className="pt-2 border-t border-slate-800/60">
                    <span className="text-slate-500 block text-[10px]">Puncak Konflik (Klimaks)</span>
                    <p className="text-slate-300 italic text-[11px] line-clamp-2 mt-0.5">{formData.climax || 'Belum diisi...'}</p>
                  </div>
                </div>

                {/* Direct Generate Action */}
                <button
                  onClick={handleGenerateStory}
                  disabled={isGeneratingStory}
                  className="mt-2 w-full bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold py-2.5 rounded-xl text-xs flex items-center justify-center gap-2 shadow-lg transition-all"
                >
                  <Wand2 className="w-4 h-4" />
                  <span>Generate Cerpen AI</span>
                </button>
              </div>
            </div>

          </div>
        )}

        {/* ==================================================================== */}
        {/* TAB 2: BOOK COVER STUDIO (GENERATOR SAMPUL BUKU EKSKLUSIF)          */}
        {/* ==================================================================== */}
        {activeTab === 'cover' && (
          <div className="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            {/* Left Controls: Design Customization Options */}
            <div className="lg:col-span-5 bg-slate-900/60 border border-slate-800 rounded-2xl p-6 flex flex-col gap-6 shadow-xl">
              <div>
                <h2 className="text-lg font-serif font-bold text-amber-400 flex items-center gap-2">
                  <Book className="w-5 h-5 text-amber-500" /> Book Cover Studio
                </h2>
                <p className="text-xs text-slate-400 mt-0.5">Rancang visual sampul cerpen eksklusif dengan ornamen sastra atau AI Art.</p>
              </div>

              {/* Theme Presets Selection */}
              <div>
                <label className="block text-xs font-semibold uppercase tracking-wider text-slate-300 mb-2">Gaya Visual Sampul</label>
                <div className="grid grid-cols-2 gap-2">
                  {[
                    { id: 'minimalist-dark', label: 'Minimalis Sastra', desc: 'Gelap, garis emas, bersih' },
                    { id: 'royal-gold', label: 'Klasik Royal Gold', desc: 'Ornamen ukiran mewat' },
                    { id: 'vintage-canvas', label: 'Vintage Canvas', desc: 'Tekstur kertas tua & tinta' },
                    { id: 'emerald-mystic', label: 'Emerald Mistik', desc: 'Nuansa hijau magis' }
                  ].map((t) => (
                    <button
                      key={t.id}
                      onClick={() => setCoverConfig({ ...coverConfig, themePreset: t.id })}
                      className={`p-3 rounded-xl border text-left transition-all ${
                        coverConfig.themePreset === t.id
                          ? 'bg-amber-500/10 border-amber-500 text-amber-300'
                          : 'bg-slate-950 border-slate-800 text-slate-400 hover:border-slate-700'
                      }`}
                    >
                      <div className="text-xs font-bold text-slate-200">{t.label}</div>
                      <div className="text-[10px] text-slate-500 mt-0.5">{t.desc}</div>
                    </button>
                  ))}
                </div>
              </div>

              {/* Font Family Selection */}
              <div>
                <label className="block text-xs font-semibold uppercase tracking-wider text-slate-300 mb-2">Tipe Tipografi Judul</label>
                <div className="grid grid-cols-3 gap-2">
                  {[
                    { id: 'serif', name: 'Merriweather Serif' },
                    { id: 'playfair', name: 'Playfair Display' },
                    { id: 'cinzel', name: 'Cinzel Classical' }
                  ].map((f) => (
                    <button
                      key={f.id}
                      onClick={() => setCoverConfig({ ...coverConfig, fontFamily: f.id })}
                      className={`p-2 rounded-lg border text-xs text-center transition-all ${
                        coverConfig.fontFamily === f.id
                          ? 'bg-amber-500/20 border-amber-500 text-amber-300 font-bold'
                          : 'bg-slate-950 border-slate-800 text-slate-400'
                      }`}
                    >
                      {f.name}
                    </button>
                  ))}
                </div>
              </div>

              {/* AI Cover Art Generation Button */}
              <div className="border-t border-slate-800/80 pt-4">
                <label className="block text-xs font-semibold uppercase tracking-wider text-slate-300 mb-1.5">Lukisan Sampul AI (Gemini Flash Image)</label>
                <p className="text-[11px] text-slate-400 mb-3">Buat lukisan latar belakang sampul berdasarkan tema cerpen secara otomatis.</p>
                <button
                  onClick={handleGenerateAICoverArt}
                  disabled={coverConfig.isGeneratingCoverArt}
                  className="w-full bg-gradient-to-r from-purple-600 to-indigo-600 hover:from-purple-500 hover:to-indigo-500 text-white font-bold py-3 rounded-xl text-xs flex items-center justify-center gap-2 transition-all shadow-lg shadow-indigo-500/20"
                >
                  <Sparkles className="w-4 h-4" />
                  <span>{coverConfig.isGeneratingCoverArt ? 'Sedang Melukis Sampul AI...' : 'Lukis Latar Sampul Dengan AI'}</span>
                </button>
              </div>

              {/* Quick Info */}
              <div className="bg-slate-950 p-3 rounded-xl border border-slate-800 text-[11px] text-slate-400">
                 Sampul ini akan disematkan secara otomatis pada halaman pertama ketika Anda mencetak (Print/PDF) atau membaca karya cerpen Anda.
              </div>
            </div>

            {/* Right Display: Live Interactive Cover Render Canvas */}
            <div className="lg:col-span-7 flex flex-col items-center justify-center">
              <div className="text-xs font-mono text-slate-400 mb-3 flex items-center gap-2">
                <Eye className="w-4 h-4 text-amber-500" /> Pratinjau Sampul Buku Sastra (3 : 4)
              </div>

              {/* Cover Canvas Display */}
              <div 
                id="short-story-cover-canvas"
                className={`w-[340px] sm:w-[380px] h-[480px] sm:h-[530px] rounded-2xl p-8 flex flex-col justify-between relative overflow-hidden shadow-2xl border-4 transition-all ${
                  coverConfig.themePreset === 'minimalist-dark'
                    ? 'bg-slate-950 border-amber-600/40 text-slate-100'
                    : coverConfig.themePreset === 'royal-gold'
                    ? 'bg-gradient-to-b from-amber-950 via-slate-950 to-slate-950 border-amber-500 text-amber-100'
                    : coverConfig.themePreset === 'vintage-canvas'
                    ? 'bg-stone-900 border-amber-800/60 text-stone-200'
                    : coverConfig.themePreset === 'emerald-mystic'
                    ? 'bg-gradient-to-b from-emerald-950 via-slate-950 to-slate-950 border-emerald-500/40 text-emerald-100'
                    : 'bg-slate-950 border-amber-500 text-slate-100'
                }`}
                style={{
                  backgroundImage: coverConfig.bgImageUrl ? `url(${coverConfig.bgImageUrl})` : 'none',
                  backgroundSize: 'cover',
                  backgroundPosition: 'center'
                }}
              >
                {/* Background Overlay for readability if image is active */}
                {coverConfig.bgImageUrl && (
                  <div className="absolute inset-0 bg-gradient-to-t from-slate-950 via-slate-950/60 to-slate-950/40 z-0"></div>
                )}

                {/* Decorative Frame Lines */}
                <div className="absolute inset-3 border border-amber-500/30 rounded-xl pointer-events-none z-10"></div>
                <div className="absolute inset-5 border border-amber-500/20 rounded-lg pointer-events-none z-10"></div>

                {/* Cover Header / Genre */}
                <div className="relative z-20 text-center">
                  <div className="inline-block text-[10px] font-mono uppercase tracking-widest text-amber-400 bg-slate-950/80 px-3 py-1 rounded-full border border-amber-500/30">
                    {formData.genre || 'Karya Sastra'}
                  </div>
                  <div className="text-[11px] font-sans text-slate-300 mt-2 tracking-wide font-medium uppercase">
                    {formData.theme}
                  </div>
                </div>

                {/* Center Title Display */}
                <div className="relative z-20 text-center my-auto px-4">
                  <h2 className={`text-2xl sm:text-3xl font-bold leading-tight drop-shadow-md text-amber-300 ${
                    coverConfig.fontFamily === 'serif' ? 'font-serif' : 'font-serif tracking-tight'
                  }`}>
                    {formData.title || "Judul Cerita Pendek"}
                  </h2>
                  <div className="w-16 h-0.5 bg-gradient-to-r from-transparent via-amber-400 to-transparent mx-auto my-4"></div>
                  <p className="text-xs italic text-slate-300 font-serif">
                    "Sebuah Antologi Sastra Penuh Makna"
                  </p>
                </div>

                {/* Cover Footer / Author Name */}
                <div className="relative z-20 text-center pt-4 border-t border-amber-500/30">
                  <div className="text-[10px] font-sans uppercase tracking-widest text-slate-400">Ditulis Oleh Maestro</div>
                  <div className="font-serif text-sm font-bold text-slate-100 tracking-wider mt-0.5">
                    {formData.author || "Nama Penulis"}
                  </div>
                </div>
              </div>
            </div>

          </div>
        )}

        {/* ==================================================================== */}
        {/* TAB 3: LITERARY READER STUDIO (TAMPILAN BACA & EKSPOR CERPEN)        */}
        {/* ==================================================================== */}
        {activeTab === 'reader' && (
          <div className="flex flex-col gap-6">

            {/* Reader Toolbar */}
            <div className="bg-slate-900/90 border border-slate-800 rounded-2xl p-4 flex flex-wrap items-center justify-between gap-4 shadow-xl">
              
              {/* Left Stats */}
              <div className="flex items-center gap-4 text-xs">
                <div className="flex items-center gap-1.5 text-slate-300">
                  <BookOpen className="w-4 h-4 text-amber-400" />
                  <span className="font-semibold">{storyStats.words}</span> Kata
                </div>
                <div className="flex items-center gap-1.5 text-slate-300 border-l border-slate-800 pl-4">
                  <Clock className="w-4 h-4 text-amber-400" />
                  <span>~{storyStats.readTime} Menit Baca</span>
                </div>
              </div>

              {/* Reader Preferences Controls */}
              <div className="flex items-center gap-3">
                {/* Font Size Adjuster */}
                <div className="flex items-center gap-2 bg-slate-950 px-3 py-1.5 rounded-xl border border-slate-800">
                  <Type className="w-3.5 h-3.5 text-slate-400" />
                  <button 
                    onClick={() => setReaderFontSize(Math.max(14, readerFontSize - 2))} 
                    className="text-xs text-slate-300 hover:text-amber-400 font-mono px-1"
                  >-</button>
                  <span className="text-xs font-mono text-amber-400 w-6 text-center">{readerFontSize}px</span>
                  <button 
                    onClick={() => setReaderFontSize(Math.min(26, readerFontSize + 2))} 
                    className="text-xs text-slate-300 hover:text-amber-400 font-mono px-1"
                  >+</button>
                </div>

                {/* Theme Selector */}
                <div className="flex items-center bg-slate-950 p-1 rounded-xl border border-slate-800 text-xs">
                  {[
                    { id: 'parchment', name: 'Kertas Parchment' },
                    { id: 'dark', name: 'Modus Gelap' },
                    { id: 'clean', name: 'Putih Bersih' }
                  ].map((t) => (
                    <button
                      key={t.id}
                      onClick={() => setReaderTheme(t.id)}
                      className={`px-2.5 py-1 rounded-lg transition-all ${
                        readerTheme === t.id ? 'bg-amber-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-slate-200'
                      }`}
                    >
                      {t.name}
                    </button>
                  ))}
                </div>

                {/* Action Buttons: Copy, Download, Print */}
                <button
                  onClick={handleCopyStory}
                  className="bg-slate-800 hover:bg-slate-700 text-slate-200 p-2.5 rounded-xl border border-slate-700/60 transition-all"
                  title="Salin Naskah Cerpen"
                >
                  <Copy className="w-4 h-4" />
                </button>

                <button
                  onClick={handleDownloadTXT}
                  className="bg-slate-800 hover:bg-slate-700 text-slate-200 p-2.5 rounded-xl border border-slate-700/60 transition-all flex items-center gap-1.5 text-xs font-medium"
                >
                  <Download className="w-4 h-4" />
                  <span className="hidden sm:inline">Download TXT</span>
                </button>

                <button
                  onClick={handlePrint}
                  className="bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold px-4 py-2.5 rounded-xl text-xs flex items-center gap-2 transition-all shadow-lg shadow-amber-500/20"
                >
                  <Printer className="w-4 h-4" />
                  <span>Cetak / Simpan PDF</span>
                </button>
              </div>

            </div>

            {/* Generated Story Content & Printable View */}
            {!generatedStory ? (
              <div className="bg-slate-900/40 border border-slate-800 rounded-2xl p-12 text-center flex flex-col items-center justify-center gap-4">
                <Feather className="w-12 h-12 text-slate-600" />
                <div>
                  <h3 className="text-base font-bold text-slate-300">Belum Ada Cerpen yang Dihasilkan</h3>
                  <p className="text-xs text-slate-500 mt-1">Silakan lengkapi kerangka cerita di Tab "1. Kerangka Cerita" dan tekan tombol Generate.</p>
                </div>
                <button
                  onClick={() => setActiveTab('framework')}
                  className="bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold px-5 py-2.5 rounded-xl text-xs mt-2"
                >
                  Buka Formulir Kerangka
                </button>
              </div>
            ) : (
              <div 
                id="printable-story-area"
                className={`rounded-2xl p-8 lg:p-16 shadow-2xl transition-all border max-w-4xl mx-auto w-full ${
                  readerTheme === 'parchment'
                    ? 'bg-[#fcfbf7] text-[#1c1917] border-[#e7e5e4]'
                    : readerTheme === 'dark'
                    ? 'bg-slate-950 text-slate-200 border-slate-800'
                    : 'bg-white text-slate-900 border-slate-200'
                }`}
              >
                {/* Print Header Cover Placeholder */}
                <div className="text-center border-b border-amber-500/30 pb-8 mb-10">
                  <span className="text-xs uppercase tracking-widest font-mono text-amber-600 block mb-2">
                    {formData.genre} • {formData.theme}
                  </span>
                  <h1 className="font-serif text-3xl sm:text-4xl lg:text-5xl font-bold tracking-tight text-slate-900 dark:text-amber-400 mb-3">
                    {formData.title}
                  </h1>
                  <p className="font-serif italic text-base text-slate-600 dark:text-slate-400">
                    Oleh Maestro Pengarang: <span className="font-semibold text-slate-900 dark:text-slate-200">{formData.author}</span>
                  </p>
                </div>

                {/* Story Main Body with Drop Cap formatting */}
                <div 
                  className="font-serif leading-relaxed space-y-6 text-justify"
                  style={{ fontSize: `${readerFontSize}px`, lineHeight: 1.8 }}
                >
                  {generatedStory.split('\n\n').map((paragraph, pIdx) => {
                    if (!paragraph.trim()) return null;

                    // If paragraph starts with heading markdown #
                    if (paragraph.startsWith('#')) {
                      return (
                        <h2 key={pIdx} className="font-serif text-2xl font-bold text-amber-600 mt-8 mb-4">
                          {paragraph.replace(/^#+\s*/, '')}
                        </h2>
                      );
                    }

                    // First story paragraph receives elegant Drop Cap
                    if (pIdx === 0 || (pIdx === 1 && generatedStory.startsWith('#'))) {
                      const firstChar = paragraph.charAt(0);
                      const restText = paragraph.slice(1);
                      return (
                        <p key={pIdx} className="relative">
                          <span className="float-left text-5xl sm:text-6xl font-bold leading-none pr-3 pt-1 text-amber-600 font-serif">
                            {firstChar}
                          </span>
                          {restText}
                        </p>
                      );
                    }

                    return (
                      <p key={pIdx} className="indent-6">
                        {paragraph}
                      </p>
                    );
                  })}
                </div>

                {/* End of Story Marker */}
                <div className="mt-16 pt-8 border-t border-amber-500/20 text-center font-serif italic text-sm text-slate-500">
                  *** SELESAI ***
                </div>
              </div>
            )}

          </div>
        )}

      </main>

      {/* Fullscreen Loading Modal during Story Generation */}
      {isGeneratingStory && (
        <div className="fixed inset-0 z-50 bg-slate-950/90 backdrop-blur-md flex flex-col items-center justify-center p-6 text-center">
          <div className="relative w-24 h-24 mb-6">
            <div className="absolute inset-0 rounded-full border-4 border-amber-500/20 animate-ping"></div>
            <div className="absolute inset-0 rounded-full border-4 border-amber-500 border-t-transparent animate-spin"></div>
            <div className="absolute inset-0 flex items-center justify-center">
              <Feather className="w-10 h-10 text-amber-400 animate-pulse" />
            </div>
          </div>

          <h2 className="font-serif text-2xl font-bold text-slate-100 mb-2">
            Sedang Mengarang Cerpen Sastra...
          </h2>
          <p className="text-xs text-amber-400 font-mono mb-6 max-w-md">
            "{generationStatusText}"
          </p>

          {/* Progress Bar */}
          <div className="w-full max-w-md bg-slate-900 border border-slate-800 h-2.5 rounded-full overflow-hidden p-0.5">
            <div 
              className="bg-gradient-to-r from-amber-600 via-amber-500 to-amber-300 h-full rounded-full transition-all duration-500"
              style={{ width: `${generationProgress}%` }}
            ></div>
          </div>

          <p className="text-[11px] text-slate-500 mt-6 italic font-serif">
            "Bahasa adalah rumah bagi jiwa pengarang."
          </p>
        </div>
      )}

      {/* Footer */}
      <footer className="mt-auto border-t border-slate-900 py-4 px-6 text-center text-xs text-slate-500 flex flex-col sm:flex-row items-center justify-between gap-2">
        <div>Aplikasi Penulis Cerpen Sastra • Studio Maestro</div>
        <div className="text-[11px] text-slate-600">Ditenagai oleh Gemini AI API (Flash Text & Image)</div>
      </footer>

    </div>
  );
}
