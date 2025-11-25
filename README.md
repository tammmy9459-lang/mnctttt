
html
import { AppView } from './types';
import { StoryMode } from './components/StoryMode';
import { RecipeMode } from './components/RecipeMode';
import { TrackerMode } from './components/TrackerMode';
import { BookOpen, ChefHat, Trophy, Home } from 'lucide-react';

const App: React.FC = () => {
  const [view, setView] = useState<AppView>(AppView.HOME);

  const renderView = () => {
    switch (view) {
      case AppView.STORY:
        return <StoryMode />;
      case AppView.RECIPE:
        return <RecipeMode />;
      case AppView.TRACKER:
        return <TrackerMode />;
      case AppView.HOME:
      default:
        return <HomeMenu setView={setView} />;
    }
  };

  return (
    <div className="min-h-screen max-w-md mx-auto bg-orange-50 relative pb-24 shadow-2xl overflow-hidden">
      {/* Header */}
      <header className="bg-white p-4 pt-8 sticky top-0 z-50 rounded-b-[30px] shadow-sm flex items-center justify-between">
        <h1 
            className="text-2xl font-black text-transparent bg-clip-text bg-gradient-to-r from-green-500 to-emerald-700 cursor-pointer"
            onClick={() => setView(AppView.HOME)}
        >
          ผักจอมพลัง 🥦
        </h1>
        {view !== AppView.HOME && (
            <button 
                onClick={() => setView(AppView.HOME)}
                className="p-2 bg-gray-100 rounded-full text-gray-500 hover:bg-gray-200 transition-colors"
            >
                <Home size={20} />
            </button>
        )}
      </header>

      {/* Main Content */}
      <main className="p-4">
        {renderView()}
      </main>

      {/* Navigation Bar (Sticky Bottom) */}
      <nav className="fixed bottom-0 w-full max-w-md bg-white border-t border-gray-100 rounded-t-[30px] p-2 px-6 shadow-[0_-5px_20px_rgba(0,0,0,0.05)] flex justify-between items-center z-50">
        <NavItem 
            icon={<Home />} 
            label="หน้าหลัก" 
            isActive={view === AppView.HOME} 
            onClick={() => setView(AppView.HOME)} 
            activeColor="text-green-500"
        />
        <NavItem 
            icon={<BookOpen />} 
            label="นิทาน" 
            isActive={view === AppView.STORY} 
            onClick={() => setView(AppView.STORY)} 
            activeColor="text-purple-500"
        />
        <NavItem 
            icon={<ChefHat />} 
            label="เมนู" 
            isActive={view === AppView.RECIPE} 
            onClick={() => setView(AppView.RECIPE)} 
            activeColor="text-orange-500"
        />
        <NavItem 
            icon={<Trophy />} 
            label="สะสมดาว" 
            isActive={view === AppView.TRACKER} 
            onClick={() => setView(AppView.TRACKER)} 
            activeColor="text-yellow-500"
        />
      </nav>
    </div>
  );
};

const HomeMenu: React.FC<{ setView: (v: AppView) => void }> = ({ setView }) => {
    return (
        <div className="space-y-6 animate-fade-in py-4">
            <div className="text-center space-y-2 mb-8">
                <img src="https://picsum.photos/400/200?random=1" alt="Happy Kids" className="w-full h-48 object-cover rounded-3xl shadow-md mb-4 opacity-90" />
                <h2 className="text-3xl font-extrabold text-slate-800">สวัสดีจ้ะคนเก่ง! 👋</h2>
                <p className="text-slate-500 font-medium">วันนี้เรามาสนุกกับผักกันเถอะ</p>
            </div>

            <div className="grid gap-4">
                <MenuCard 
                    title="เล่านิทานฮีโร่" 
                    desc="ถ่ายรูปผักแล้วแปลงร่างเป็นฮีโร่!" 
                    icon="🦸‍♂️" 
                    color="bg-purple-100 text-purple-700"
                    onClick={() => setView(AppView.STORY)}
                />
                <MenuCard 
                    title="เมนูวิเศษ" 
                    desc="เสกผักให้เป็นอาหารแสนอร่อย" 
                    icon="🍳" 
                    color="bg-orange-100 text-orange-700"
                    onClick={() => setView(AppView.RECIPE)}
                />
                 <MenuCard 
                    title="บันทึกคนเก่ง" 
                    desc="สะสมดาวจากการกินผัก" 
                    icon="⭐" 
                    color="bg-green-100 text-green-700"
                    onClick={() => setView(AppView.TRACKER)}
                />
            </div>
        </div>
    )
}

const MenuCard: React.FC<{ title: string; desc: string; icon: string; color: string; onClick: () => void }> = ({ title, desc, icon, color, onClick }) => (
    <button 
        onClick={onClick}
        className={`${color} w-full p-6 rounded-3xl flex items-center gap-4 text-left transition-transform active:scale-95 shadow-sm border-2 border-white`}
    >
        <div className="text-4xl bg-white w-16 h-16 rounded-2xl flex items-center justify-center shadow-sm">
            {icon}
        </div>
        <div>
            <h3 className="text-xl font-bold">{title}</h3>
            <p className="text-sm opacity-80 font-medium">{desc}</p>
        </div>
    </button>
)

const NavItem: React.FC<{ icon: React.ReactNode; label: string; isActive: boolean; onClick: () => void; activeColor: string }> = ({ icon, label, isActive, onClick, activeColor }) => (
    <button 
        onClick={onClick}
        className={`flex flex-col items-center gap-1 p-2 transition-all ${isActive ? activeColor : 'text-gray-400 hover:text-gray-600'}`}
    >
        <div className={`transition-transform ${isActive ? 'scale-110' : ''}`}>
            {icon}
        </div>
        <span className="text-[10px] font-bold">{label}</span>
    </button>
)

export default App;
