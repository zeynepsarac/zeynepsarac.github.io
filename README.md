import React, { useEffect, useState } from 'react';
import { MapPin, Bell, Settings, User, MessageCircle } from 'lucide-react';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';
import { GoogleSpreadsheet } from 'google-spreadsheet';
import dynamic from 'next/dynamic';

// Dynamically import the map component (for client-side rendering only)
const TurkeyMap = dynamic(() => import('turkey-map-react'), { ssr: false });

export default function BooksphereLayout() {
  const [view, setView] = useState('home');
  const [cityData, setCityData] = useState({});

  // Dummy city data (to be replaced with fetched data)
  useEffect(() => {
    // TODO: Replace with actual fetch from Google Sheets using API
    const dummy = {
      'İstanbul': 153,
      'Ankara': 89,
      'İzmir': 76,
      'Bursa': 45,
      'Antalya': 62
    };
    setCityData(dummy);
  }, []);

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Navbar */}
      <div className="w-full flex items-center justify-between bg-white shadow-md p-4 fixed top-0 z-50">
        <div className="flex items-center space-x-2">
          <img src="/logo.svg" alt="Booksphere Logo" className="h-8 w-8" />
          <span className="text-xl font-bold text-gray-800">Booksphere</span>
        </div>
        <div className="flex items-center space-x-6">
          <Button variant="ghost" onClick={() => setView('home')}>Anasayfa</Button>
          <Button variant="ghost" onClick={() => setView('map')}>Harita</Button>
          <Input placeholder="Kitap ara..." className="w-64" />
        </div>
        <div className="flex items-center space-x-4">
          <Button variant="ghost" size="icon"><User className="w-5 h-5" /></Button>
          <Button variant="ghost" size="icon"><MessageCircle className="w-5 h-5" /></Button>
          <Button variant="ghost" size="icon"><Settings className="w-5 h-5" /></Button>
          <Button variant="ghost" size="icon"><Bell className="w-5 h-5" /></Button>
        </div>
      </div>

      {/* Main content */}
      <div className="pt-24 max-w-4xl mx-auto p-4">
        {view === 'home' && (
          <div className="space-y-6">
            <Card className="bg-white shadow-sm">
              <CardContent className="p-4">
                <textarea
                  placeholder="Hangi kitabı okuyorsun? Düşüncelerini paylaş..."
                  className="w-full h-24 p-2 border rounded-md resize-none focus:outline-none focus:ring-2 focus:ring-blue-400"
                />
                <div className="flex justify-end mt-2">
                  <Button>Paylaş</Button>
                </div>
              </CardContent>
            </Card>

            <Card className="bg-white shadow-sm">
              <CardContent className="p-4">
                <div className="font-semibold text-gray-800">Ayşe Yılmaz</div>
                <div className="text-gray-600 text-sm">📖 "Kürk Mantolu Madonna" okumaya başladım. Sabah kahvesiyle harika gidiyor!</div>
                <div className="text-gray-400 text-xs mt-2">5 dakika önce</div>
              </CardContent>
            </Card>

            <Card className="bg-white shadow-sm">
              <CardContent className="p-4">
                <div className="font-semibold text-gray-800">Mehmet Kara</div>
                <div className="text-gray-600 text-sm">📚 Bugün "Simyacı"yı bitirdim. Paulo Coelho'nun dili çok akıcıydı.</div>
                <div className="text-gray-400 text-xs mt-2">1 saat önce</div>
              </CardContent>
            </Card>
          </div>
        )}

        {view === 'map' && (
          <div className="bg-white shadow-md rounded-lg p-6">
            <h2 className="text-xl font-semibold text-gray-800 mb-4">Türkiye Okuma Haritası</h2>
            <TurkeyMap
              onClick={({ name }) => alert(`${name} şehrine tıklanıldı.`)}
              customStyle={{
                backgroundColor: '#f3f4f6',
                border: '1px solid #ccc'
              }}
              cityWrapper={(cityName) => (
                <div className="relative">
                  <span className="absolute text-xs text-blue-700 font-semibold -top-2 -left-2">
                    {cityData[cityName] || 0} kitap
                  </span>
                </div>
              )}
            />
          </div>
        )}
      </div>
    </div>
  );
}
