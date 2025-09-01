<img width="1912" height="921" alt="image" src="https://github.com/user-attachments/assets/f26ae8a7-a164-4df6-abd7-f70b8fa46274" />
   


// App.tsx
import React from "react";
import {
  Home,
  Settings,
  Grid,
  MessageSquare,
  Video,
  ClipboardList,
  Search,
  ShoppingBag,
  Users,
  Headphones,
  BarChart3,
  Bell,
} from "lucide-react";

/** Layout primitives */
function SidebarIcon({ children }: { children: React.ReactNode }) {
  return (
    <button className="w-11 h-11 grid place-items-center rounded-xl text-slate-500 hover:text-emerald-600 hover:bg-emerald-50 transition">
      {children}
    </button>
  );
}

function TopAvatar() {
  return (
    <div className="flex items-center gap-3">
      <button className="relative w-10 h-10 grid place-items-center rounded-full text-slate-500 hover:text-emerald-600 hover:bg-emerald-50 transition">
        <Bell size={18} />
      </button>
      <div className="w-10 h-10 rounded-full bg-slate-200 grid place-items-center text-slate-600 font-semibold">
        RG
      </div>
      <div className="text-right leading-tight">
        <div className="text-slate-700 text-sm font-semibold">Rafael G.</div>
        <div className="text-slate-400 text-xs">Agência 01</div>
      </div>
    </div>
  );
}

function Logo() {
  return (
    <div className="flex items-center gap-2 select-none">
      <div className="w-7 h-7 rounded-full bg-emerald-600 grid place-items-center text-white text-xs font-bold">
        S
      </div>
      <span className="text-emerald-700 font-semibold tracking-tight">Sicredi</span>
    </div>
  );
}

/** Page */
export default function App() {
  const categories: any[] = []; // vazio para reproduzir o estado “Sem resultados encontrados”

  return (
    <div className="min-h-screen bg-slate-100 text-slate-800">
      {/* Top bar */}
      <header className="h-14 px-5 border-b border-slate-200 bg-white flex items-center justify-between">
        <Logo />
        <TopAvatar />
      </header>

      {/* Body */}
      <div className="flex">
        {/* Sidebar */}
        <aside className="w-[60px] shrink-0 border-r border-slate-200 bg-white pt-3 flex flex-col items-center gap-2">
          <SidebarIcon><Home size={18} /></SidebarIcon>
          <SidebarIcon><Grid size={18} /></SidebarIcon>
          <SidebarIcon><Settings size={18} /></SidebarIcon>
          <SidebarIcon><MessageSquare size={18} /></SidebarIcon>
          <SidebarIcon><Video size={18} /></SidebarIcon>
          <SidebarIcon><ClipboardList size={18} /></SidebarIcon>
          <SidebarIcon><Search size={18} /></SidebarIcon>
          <SidebarIcon><ShoppingBag size={18} /></SidebarIcon>
          <SidebarIcon><Users size={18} /></SidebarIcon>
          <SidebarIcon><Headphones size={18} /></SidebarIcon>
          <div className="mt-auto mb-3">
            <SidebarIcon><BarChart3 size={18} /></SidebarIcon>
          </div>
        </aside>

        {/* Main */}
        <main className="flex-1 p-6">
          {/* Card “Categorias” */}
          <div className="rounded-xl border border-slate-200 bg-white shadow-sm">
            {/* Card header */}
            <div className="flex items-center justify-between px-5 py-4">
              <h2 className="text-lg font-semibold">Categorias</h2>

              <div className="relative">
                <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" size={16} />
                <input
                  placeholder="Procurar..."
                  className="pl-9 pr-3 py-2 w-64 rounded-lg border border-slate-200 bg-white text-sm outline-none focus:ring-2 focus:ring-emerald-200 focus:border-emerald-400"
                />
              </div>
            </div>

            {/* Content */}
            <div className="px-5 pb-4">
              <div className="rounded-lg border border-slate-200 bg-white">
                <div className="p-8 text-sm text-slate-400">
                  {categories.length === 0 ? "Sem resultados encontrados" : null}
                </div>

                {/* Pagination */}
                <div className="px-4 py-3 border-t border-slate-200">
                  <div className="flex items-center justify-center gap-4 text-slate-400 text-xs">
                    <button className="hover:text-emerald-600 transition">«</button>
                    <button className="hover:text-emerald-600 transition">‹</button>
                    <span className="inline-flex items-center gap-2">
                      <span className="w-1 h-1 rounded-full bg-slate-300" />
                      <span className="w-1 h-1 rounded-full bg-slate-300" />
                      <span className="w-1 h-1 rounded-full bg-slate-300" />
                    </span>
                    <button className="hover:text-emerald-600 transition">›</button>
                    <button className="hover:text-emerald-600 transition">»</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>
  );
}