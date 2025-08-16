import React, { useState, useEffect } from 'react';

// Mock data for exchanges (in production, this would come from API)
const mockExchanges = [
  {
    id: 1,
    name: 'Binance',
    logo: 'https://placehold.co/50x50/f3ba2f/ffffff?text=BNB',
    rating: 4.8,
    makerFee: 0.1,
    takerFee: 0.1,
    vipTiers: [
      { level: 'VIP 0', maker: 0.1, taker: 0.1, volume: '0 BTC' },
      { level: 'VIP 1', maker: 0.09, taker: 0.1, volume: '50 BTC' },
      { level: 'VIP 2', maker: 0.08, taker: 0.09, volume: '250 BTC' },
    ],
    withdrawalFees: {
      BTC: { network: 'BTC', fee: 0.0005, minAmount: 0.001 },
      ETH: { network: 'ERC20', fee: 0.01, minAmount: 0.01 },
      USDT: [
        { network: 'TRC20', fee: 1.0, minAmount: 10 },
        { network: 'ERC20', fee: 15.0, minAmount: 10 },
        { network: 'BEP20', fee: 1.0, minAmount: 10 }
      ]
    },
    depositSupported: ['BTC', 'ETH', 'USDT', 'BNB', 'SOL', 'XRP'],
    affiliateLink: 'https://binance.com/signup',
    reviewCount: 1250,
    description: 'World\'s largest cryptocurrency exchange by volume',
    lastUpdated: new Date().toISOString()
  },
  {
    id: 2,
    name: 'KuCoin',
    logo: 'https://placehold.co/50x50/3a4ef5/ffffff?text=KC',
    rating: 4.6,
    makerFee: 0.1,
    takerFee: 0.1,
    vipTiers: [
      { level: 'Standard', maker: 0.1, taker: 0.1, volume: '0 USDT' },
      { level: 'Premium', maker: 0.08, taker: 0.1, volume: '100k USDT' },
      { level: 'VIP', maker: 0.06, taker: 0.08, volume: '1M USDT' },
    ],
    withdrawalFees: {
      BTC: { network: 'BTC', fee: 0.0005, minAmount: 0.001 },
      ETH: { network: 'ERC20', fee: 0.01, minAmount: 0.01 },
      USDT: [
        { network: 'TRC20', fee: 1.0, minAmount: 5 },
        { network: 'ERC20', fee: 20.0, minAmount: 10 },
        { network: 'Polygon', fee: 1.5, minAmount: 10 }
      ]
    },
    depositSupported: ['BTC', 'ETH', 'USDT', 'KCS', 'ADA', 'DOT'],
    affiliateLink: 'https://kucoin.com/signup',
    reviewCount: 890,
    description: 'Popular exchange with wide altcoin selection',
    lastUpdated: new Date().toISOString()
  },
  {
    id: 3,
    name: 'Bybit',
    logo: 'https://placehold.co/50x50/1e293b/ffffff?text=BY',
    rating: 4.7,
    makerFee: 0.01,
    takerFee: 0.06,
    vipTiers: [
      { level: 'Standard', maker: 0.01, taker: 0.06, volume: '0 USDT' },
      { level: 'VIP 1', maker: -0.005, taker: 0.055, volume: '500k USDT' },
      { level: 'VIP 2', maker: -0.01, taker: 0.05, volume: '2M USDT' },
    ],
    withdrawalFees: {
      BTC: { network: 'BTC', fee: 0.0005, minAmount: 0.001 },
      ETH: { network: 'ERC20', fee: 0.01, minAmount: 0.01 },
      USDT: [
        { network: 'TRC20', fee: 1.0, minAmount: 10 },
        { network: 'ERC20', fee: 15.0, minAmount: 10 },
        { network: 'BEP20', fee: 1.0, minAmount: 10 }
      ]
    },
    depositSupported: ['BTC', 'ETH', 'USDT', 'XRP', 'SOL', 'DOGE'],
    affiliateLink: 'https://bybit.com/signup',
    reviewCount: 1100,
    description: 'Leading derivatives exchange with competitive fees',
    lastUpdated: new Date().toISOString()
  },
  {
    id: 4,
    name: 'OKX',
    logo: 'https://placehold.co/50x50/f04a21/ffffff?text=OK',
    rating: 4.5,
    makerFee: 0.1,
    takerFee: 0.15,
    vipTiers: [
      { level: 'OKX Level 1', maker: 0.1, taker: 0.15, volume: '0 USDT' },
      { level: 'OKX Level 2', maker: 0.08, taker: 0.12, volume: '100k USDT' },
      { level: 'OKX Level 3', maker: 0.06, taker: 0.1, volume: '1M USDT' },
    ],
    withdrawalFees: {
      BTC: { network: 'BTC', fee: 0.0005, minAmount: 0.001 },
      ETH: { network: 'ERC20', fee: 0.01, minAmount: 0.01 },
      USDT: [
        { network: 'TRC20', fee: 1.0, minAmount: 10 },
        { network: 'ERC20', fee: 15.0, minAmount: 10 },
        { network: 'BEP20', fee: 1.0, minAmount: 10 }
      ]
    },
    depositSupported: ['BTC', 'ETH', 'USDT', 'OKB', 'LTC', 'ATOM'],
    affiliateLink: 'https://okx.com/signup',
    reviewCount: 750,
    description: 'Global exchange with advanced trading features',
    lastUpdated: new Date().toISOString()
  }
];

const mockReviews = [
  {
    id: 1,
    exchangeId: 1,
    user: 'CryptoTrader88',
    rating: 5,
    comment: 'Binance has the lowest fees and fastest withdrawals. TRC20 network is super cheap!',
    date: '2024-01-15',
    helpful: 45
  },
  {
    id: 2,
    exchangeId: 1,
    user: 'DeFiInvestor',
    rating: 4,
    comment: 'Great platform but customer support could be better. Fees are competitive though.',
    date: '2024-01-10',
    helpful: 32
  },
  {
    id: 3,
    exchangeId: 2,
    user: 'AltcoinHunter',
    rating: 5,
    comment: 'KuCoin has the best altcoin selection. Withdrawal fees are reasonable.',
    date: '2024-01-12',
    helpful: 28
  }
];

const App = () => {
  const [darkMode, setDarkMode] = useState(false);
  const [activePage, setActivePage] = useState('home');
  const [selectedExchange, setSelectedExchange] = useState(null);
  const [selectedCoin, setSelectedCoin] = useState('USDT');
  const [searchTerm, setSearchTerm] = useState('');
  const [user, setUser] = useState(null);
  const [premiumUser, setPremiumUser] = useState(false);
  const [showLoginModal, setShowLoginModal] = useState(false);
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  useEffect(() => {
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    setDarkMode(prefersDark);
  }, []);

  const filteredExchanges = mockExchanges.filter(exchange =>
    exchange.name.toLowerCase().includes(searchTerm.toLowerCase())
  );

  const getCoinNetworks = (coin) => {
    const networks = new Map();
    
    mockExchanges.forEach(exchange => {
      const coinFees = exchange.withdrawalFees[coin];
      if (coinFees && Array.isArray(coinFees)) {
        coinFees.forEach(fee => {
          if (!networks.has(fee.network)) {
            networks.set(fee.network, []);
          }
          networks.get(fee.network).push({
            exchange: exchange.name,
            fee: fee.fee,
            minAmount: fee.minAmount,
            logo: exchange.logo
          });
        });
      }
    });
    
    return Array.from(networks.entries()).map(([network, fees]) => ({
      network,
      fees: fees.sort((a, b) => a.fee - b.fee)
    }));
  };

  const coinNetworks = getCoinNetworks(selectedCoin);

  const toggleDarkMode = () => {
    setDarkMode(!darkMode);
  };

  const handleExchangeClick = (exchange) => {
    setSelectedExchange(exchange);
    setActivePage('exchange');
  };

  const handleLogin = (e) => {
    e.preventDefault();
    setUser({ name: 'John Doe', email: email });
    setPremiumUser(email === 'premium@example.com');
    setShowLoginModal(false);
    setEmail('');
    setPassword('');
  };

  const handleLogout = () => {
    setUser(null);
    setPremiumUser(false);
  };

  const ExchangeCard = ({ exchange }) => (
    <div className={`rounded-xl p-6 transition-all duration-300 hover:shadow-lg ${
      darkMode ? 'bg-gray-800 hover:bg-gray-750 border border-gray-700' : 'bg-white hover:shadow-xl border border-gray-200'
    }`}>
      <div className="flex items-center justify-between mb-4">
        <div className="flex items-center space-x-3">
          <img src={exchange.logo} alt={exchange.name} className="w-12 h-12 rounded-lg" />
          <div>
            <h3 className={`text-xl font-bold ${darkMode ? 'text-white' : 'text-gray-900'}`}>{exchange.name}</h3>
            <p className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>{exchange.description}</p>
          </div>
        </div>
        <div className="text-right">
          <div className="flex items-center space-x-1">
            <svg className="w-4 h-4 text-yellow-400 fill-current" viewBox="0 0 20 20">
              <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
            </svg>
            <span className={`font-semibold ${darkMode ? 'text-white' : 'text-gray-900'}`}>{exchange.rating}</span>
          </div>
          <span className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>{exchange.reviewCount} reviews</span>
        </div>
      </div>

      <div className={`grid grid-cols-2 gap-4 mb-4 p-4 rounded-lg ${darkMode ? 'bg-gray-750' : 'bg-gray-50'}`}>
        <div>
          <p className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>Maker Fee</p>
          <p className={`text-lg font-bold ${darkMode ? 'text-green-400' : 'text-green-600'}`}>{exchange.makerFee}%</p>
        </div>
        <div>
          <p className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>Taker Fee</p>
          <p className={`text-lg font-bold ${darkMode ? 'text-red-400' : 'text-red-600'}`}>{exchange.takerFee}%</p>
        </div>
      </div>

      <div className="flex space-x-2 mb-4">
        {['BTC', 'ETH', 'USDT'].map(coin => (
          <span key={coin} className={`px-2 py-1 text-xs rounded-full ${
            darkMode ? 'bg-gray-700 text-gray-300' : 'bg-gray-200 text-gray-800'
          }`}>
            {coin}
          </span>
        ))}
      </div>

      <div className="flex space-x-2">
        <button
          onClick={() => handleExchangeClick(exchange)}
          className={`flex-1 py-2 px-4 rounded-lg font-semibold transition-colors ${
            darkMode 
              ? 'bg-blue-600 hover:bg-blue-700 text-white' 
              : 'bg-blue-500 hover:bg-blue-600 text-white'
          }`}
        >
          View Details
        </button>
        <a
          href={exchange.affiliateLink}
          target="_blank"
          rel="noopener noreferrer"
          className={`py-2 px-4 rounded-lg font-semibold transition-colors flex items-center justify-center ${
            darkMode 
              ? 'bg-green-600 hover:bg-green-700 text-white' 
              : 'bg-green-500 hover:bg-green-600 text-white'
          }`}
        >
          <span>Sign Up</span>
          <svg className="w-4 h-4 ml-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
          </svg>
        </a>
      </div>
    </div>
  );

  const HomePage = () => (
    <div className="space-y-8">
      <div className={`text-center py-12 rounded-2xl mb-8 ${
        darkMode ? 'bg-gradient-to-br from-gray-800 to-gray-900' : 'bg-gradient-to-br from-blue-50 to-indigo-100'
      }`}>
        <h1 className={`text-4xl md:text-5xl font-bold mb-4 ${
          darkMode ? 'text-white' : 'text-gray-900'
        }`}>
          Crypto Exchange Fee Comparison
        </h1>
        <p className={`text-xl mb-8 ${darkMode ? 'text-gray-300' : 'text-gray-600'}`}>
          Compare trading and withdrawal fees across top exchanges
        </p>
        
        <div className={`max-w-md mx-auto p-1 rounded-xl ${
          darkMode ? 'bg-gray-700' : 'bg-white shadow-lg'
        }`}>
          <div className="flex items-center">
            <svg className="w-5 h-5 ml-3 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
            <input
              type="text"
              placeholder="Search exchanges..."
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
              className={`flex-1 py-3 px-4 rounded-xl outline-none ${
                darkMode 
                  ? 'bg-gray-700 text-white placeholder-gray-400' 
                  : 'bg-white text-gray-900 placeholder-gray-500'
              }`}
            />
          </div>
        </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div className={`p-6 rounded-xl ${darkMode ? 'bg-gray-800' : 'bg-white'} shadow-lg`}>
          <div className="flex items-center">
            <div className={`p-3 rounded-lg ${darkMode ? 'bg-blue-900' : 'bg-blue-100'}`}>
              <svg className="w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 7h8m0 0v8m0-8l-8 8-4-4-8 8" />
              </svg>
            </div>
            <div className="ml-4">
              <p className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>Exchanges Compared</p>
              <p className="text-2xl font-bold">4</p>
            </div>
          </div>
        </div>

        <div className={`p-6 rounded-xl ${darkMode ? 'bg-gray-800' : 'bg-white'} shadow-lg`}>
          <div className="flex items-center">
            <div className={`p-3 rounded-lg ${darkMode ? 'bg-green-900' : 'bg-green-100'}`}>
              <svg className="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1" />
              </svg>
            </div>
            <div className="ml-4">
              <p className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>Average Maker Fee</p>
              <p className="text-2xl font-bold">0.07%</p>
            </div>
          </div>
        </div>

        <div className={`p-6 rounded-xl ${darkMode ? 'bg-gray-800' : 'bg-white'} shadow-lg`}>
          <div className="flex items-center">
            <div className={`p-3 rounded-lg ${darkMode ? 'bg-purple-900' : 'bg-purple-100'}`}>
              <svg className="w-6 h-6 text-purple-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z" />
              </svg>
            </div>
            <div className="ml-4">
              <p className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>USDT TRC20 Avg Fee</p>
              <p className="text-2xl font-bold">$1.00</p>
            </div>
          </div>
        </div>
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        {filteredExchanges.map(exchange => (
          <ExchangeCard key={exchange.id} exchange={exchange} />
        ))}
      </div>

      {filteredExchanges.length === 0 && (
        <div className={`text-center py-12 rounded-xl ${darkMode ? 'bg-gray-800' : 'bg-gray-50'}`}>
          <svg className="w-12 h-12 mx-auto mb-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
          <h3 className={`text-xl font-semibold ${darkMode ? 'text-white' : 'text-gray-900'}`}>No exchanges found</h3>
          <p className={`${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>Try adjusting your search terms</p>
        </div>
      )}
    </div>
  );

  const ExchangeDetailPage = () => {
    if (!selectedExchange) return null;

    const reviews = mockReviews.filter(review => review.exchangeId === selectedExchange.id);

    return (
      <div className="space-y-6">
        <button
          onClick={() => setActivePage('home')}
          className={`flex items-center space-x-2 mb-6 px-4 py-2 rounded-lg ${
            darkMode 
              ? 'bg-gray-800 hover:bg-gray-700 text-white' 
              : 'bg-white hover:bg-gray-100 text-gray-900 shadow'
          }`}
        >
          ← Back to Exchanges
        </button>

        <div className={`rounded-2xl overflow-hidden ${
          darkMode ? 'bg-gray-800 border border-gray-700' : 'bg-white shadow-xl'
        }`}>
          <div className={`p-8 ${darkMode ? 'bg-gray-900' : 'bg-gradient-to-r from-blue-600 to-indigo-700'}`}>
            <div className="flex flex-col md:flex-row md:items-center md:justify-between">
              <div className="flex items-center space-x-4 mb-4 md:mb-0">
                <img src={selectedExchange.logo} alt={selectedExchange.name} className="w-16 h-16 rounded-xl" />
                <div>
                  <h1 className="text-3xl font-bold text-white">{selectedExchange.name}</h1>
                  <p className="text-blue-100">{selectedExchange.description}</p>
                </div>
              </div>
              <div className="text-right">
                <div className="flex items-center justify-end space-x-2 mb-2">
                  <svg className="w-5 h-5 text-yellow-400 fill-current" viewBox="0 0 20 20">
                    <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
                  </svg>
                  <span className="text-xl font-bold text-white">{selectedExchange.rating}</span>
                </div>
                <a
                  href={selectedExchange.affiliateLink}
                  target="_blank"
                  rel="noopener noreferrer"
                  className="inline-flex items-center space-x-2 bg-white text-blue-600 px-6 py-3 rounded-lg font-semibold hover:bg-blue-50 transition-colors"
                >
                  <span>Sign Up Now</span>
                  <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                  </svg>
                </a>
              </div>
            </div>
          </div>

          <div className="p-8">
            <div className="mb-8">
              <h2 className={`text-2xl font-bold mb-6 ${darkMode ? 'text-white' : 'text-gray-900'}`}>Trading Fees</h2>
              <div className={`grid grid-cols-1 md:grid-cols-2 gap-6 mb-6`}>
                <div className={`p-6 rounded-xl ${darkMode ? 'bg-gray-750' : 'bg-gray-50'}`}>
                  <h3 className={`text-lg font-semibold mb-4 ${darkMode ? 'text-white' : 'text-gray-900'}`}>Standard Fees</h3>
                  <div className="space-y-3">
                    <div className="flex justify-between">
                      <span className={darkMode ? 'text-gray-300' : 'text-gray-600'}>Maker Fee</span>
                      <span className={`font-semibold ${darkMode ? 'text-green-400' : 'text-green-600'}`}>
                        {selectedExchange.makerFee}%
                      </span>
                    </div>
                    <div className="flex justify-between">
                      <span className={darkMode ? 'text-gray-300' : 'text-gray-600'}>Taker Fee</span>
                      <span className={`font-semibold ${darkMode ? 'text-red-400' : 'text-red-600'}`}>
                        {selectedExchange.takerFee}%
                      </span>
                    </div>
                  </div>
                </div>

                <div className={`p-6 rounded-xl ${darkMode ? 'bg-gray-750' : 'bg-gray-50'}`}>
                  <h3 className={`text-lg font-semibold mb-4 ${darkMode ? 'text-white' : 'text-gray-900'}`}>VIP Tiers</h3>
                  <div className="space-y-2 text-sm">
                    {selectedExchange.vipTiers.map((tier, index) => (
                      <div key={index} className={`p-3 rounded-lg ${
                        darkMode ? 'bg-gray-700' : 'bg-white shadow-sm'
                      }`}>
                        <div className="flex justify-between mb-1">
                          <span className="font-medium">{tier.level}</span>
                          <span className={darkMode ? 'text-gray-300' : 'text-gray-600'}>
                            ≥ {tier.volume}
                          </span>
                        </div>
                        <div className="flex justify-between text-xs">
                          <span className={darkMode ? 'text-gray-400' : 'text-gray-500'}>
                            Maker: {tier.maker}%
                          </span>
                          <span className={darkMode ? 'text-gray-400' : 'text-gray-500'}>
                            Taker: {tier.taker}%
                          </span>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            </div>

            <div className="mb-8">
              <h2 className={`text-2xl font-bold mb-6 ${darkMode ? 'text-white' : 'text-gray-900'}`}>Withdrawal Fees</h2>
              <div className={`overflow-x-auto rounded-xl ${darkMode ? 'bg-gray-750' : 'bg-gray-50'}`}>
                <table className="w-full">
                  <thead>
                    <tr className={`text-left ${darkMode ? 'text-gray-300' : 'text-gray-700'}`}>
                      <th className="p-4 font-semibold">Coin</th>
                      <th className="p-4 font-semibold">Network</th>
                      <th className="p-4 font-semibold">Fee</th>
                      <th className="p-4 font-semibold">Min Amount</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-gray-700">
                    {Object.entries(selectedExchange.withdrawalFees).map(([coin, feeData]) => {
                      if (Array.isArray(feeData)) {
                        return feeData.map((networkFee, index) => (
                          <tr key={`${coin}-${index}`} className={darkMode ? 'hover:bg-gray-700' : 'hover:bg-gray-100'}>
                            <td className="p-4 font-medium">{coin}</td>
                            <td className="p-4">{networkFee.network}</td>
                            <td className="p-4">{networkFee.fee}</td>
                            <td className="p-4">{networkFee.minAmount}</td>
                          </tr>
                        ));
                      } else {
                        return (
                          <tr key={coin} className={darkMode ? 'hover:bg-gray-700' : 'hover:bg-gray-100'}>
                            <td className="p-4 font-medium">{coin}</td>
                            <td className="p-4">{feeData.network}</td>
                            <td className="p-4">{feeData.fee}</td>
                            <td className="p-4">{feeData.minAmount}</td>
                          </tr>
                        );
                      }
                    })}
                  </tbody>
                </table>
              </div>
            </div>

            <div className="mb-8">
              <h2 className={`text-2xl font-bold mb-6 ${darkMode ? 'text-white' : 'text-gray-900'}`}>Deposit Networks</h2>
              <div className={`p-6 rounded-xl ${darkMode ? 'bg-gray-750' : 'bg-gray-50'}`}>
                <div className="flex flex-wrap gap-2">
                  {selectedExchange.depositSupported.map(coin => (
                    <span key={coin} className={`px-3 py-2 rounded-lg text-sm font-medium ${
                      darkMode 
                        ? 'bg-blue-900 text-blue-200' 
                        : 'bg-blue-100 text-blue-800'
                    }`}>
                      {coin}
                    </span>
                  ))}
                </div>
              </div>
            </div>

            <div>
              <h2 className={`text-2xl font-bold mb-6 ${darkMode ? 'text-white' : 'text-gray-900'}`}>
                User Reviews ({reviews.length})
              </h2>
              <div className="space-y-4">
                {reviews.map(review => (
                  <div key={review.id} className={`p-6 rounded-xl ${
                    darkMode ? 'bg-gray-750' : 'bg-gray-50'
                  }`}>
                    <div className="flex items-center justify-between mb-3">
                      <div className="flex items-center space-x-3">
                        <div className={`w-10 h-10 rounded-full flex items-center justify-center ${
                          darkMode ? 'bg-gray-700' : 'bg-gray-200'
                        }`}>
                          {review.user.charAt(0)}
                        </div>
                        <div>
                          <p className={`font-semibold ${darkMode ? 'text-white' : 'text-gray-900'}`}>
                            {review.user}
                          </p>
                          <div className="flex items-center space-x-1">
                            {[...Array(5)].map((_, i) => (
                              <svg
                                key={i}
                                className={`w-4 h-4 ${
                                  i < review.rating 
                                    ? 'text-yellow-400 fill-current' 
                                    : darkMode ? 'text-gray-600' : 'text-gray-300'
                                }`}
                                viewBox="0 0 20 20"
                              >
                                <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
                              </svg>
                            ))}
                          </div>
                        </div>
                      </div>
                      <span className={`text-sm ${darkMode ? 'text-gray-400' : 'text-gray-500'}`}>
                        {review.date}
                      </span>
                    </div>
                    <p className={darkMode ? 'text-gray-300' : 'text-gray-700'}>
                      {review.comment}
                    </p>
                    <div className={`mt-3 text-sm ${darkMode ? 'text-gray-400' : 'text-gray-500'}`}>
                      {review.helpful} found this helpful
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        </div>
      </div>
    );
  };

  const WalletComparisonPage = () => (
    <div className="space-y-6">
      <div className="flex flex-col md:flex-row md:items-center md:justify-between mb-8">
        <div>
          <h1 className={`text-3xl font-bold ${darkMode ? 'text-white' : 'text-gray-900'}`}>
            Wallet Fee Comparison
          </h1>
          <p className={`mt-2 ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>
            Compare withdrawal fees across exchanges for different networks
          </p>
        </div>
        <button
          onClick={() => setActivePage('home')}
          className={`mt-4 md:mt-0 px-4 py-2 rounded-lg ${
            darkMode 
              ? 'bg-gray-800 hover:bg-gray-700 text-white' 
              : 'bg-white hover:bg-gray-100 text-gray-900 shadow'
          }`}
        >
          ← Back to Home
        </button>
      </div>

      <div className={`p-6 rounded-xl mb-8 ${darkMode ? 'bg-gray-800' : 'bg-white shadow-lg'}`}>
        <h2 className={`text-lg font-semibold mb-4 ${darkMode ? 'text-white' : 'text-gray-900'}`}>
          Select Coin
        </h2>
        <div className="flex flex-wrap gap-2">
          {['BTC', 'ETH', 'USDT', 'BNB'].map(coin => (
            <button
              key={coin}
              onClick={() => setSelectedCoin(coin)}
              className={`px-4 py-2 rounded-lg font-medium transition-colors ${
                selectedCoin === coin
                  ? darkMode
                    ? 'bg-blue-600 text-white'
                    : 'bg-blue-500 text-white'
                  : darkMode
                    ? 'bg-gray-700 text-gray-300 hover:bg-gray-600'
                    : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
              }`}
            >
              {coin}
            </button>
          ))}
        </div>
      </div>

      <div className={`rounded-xl overflow-hidden ${darkMode ? 'bg-gray-800 border border-gray-700' : 'bg-white shadow-xl'}`}>
        <div className={`px-6 py-4 border-b ${darkMode ? 'border-gray-700' : 'border-gray-200'}`}>
          <h2 className={`text-xl font-bold ${darkMode ? 'text-white' : 'text-gray-900'}`}>
            {selectedCoin} Withdrawal Fees by Network
          </h2>
        </div>
        
        <div className="overflow-x-auto">
          <table className="w-full">
            <thead>
              <tr className={`text-left ${darkMode ? 'text-gray-300' : 'text-gray-700'}`}>
                <th className="p-4 font-semibold">Network</th>
                <th className="p-4 font-semibold">Exchange</th>
                <th className="p-4 font-semibold">Fee</th>
                <th className="p-4 font-semibold">Min Amount</th>
                <th className="p-4 font-semibold">Best Value</th>
              </tr>
            </thead>
            <tbody className="divide-y divide-gray-700">
              {coinNetworks.map((networkData, networkIndex) => (
                <React.Fragment key={networkData.network}>
                  {networkData.fees.map((fee, feeIndex) => (
                    <tr 
                      key={`${networkData.network}-${feeIndex}`} 
                      className={`
                        ${darkMode ? 'hover:bg-gray-700' : 'hover:bg-gray-50'}
                        ${feeIndex === 0 ? 'bg-green-50 dark:bg-green-900/20' : ''}
                      `}
                    >
                      {feeIndex === 0 && (
                        <td 
                          rowSpan={networkData.fees.length} 
                          className={`p-4 font-semibold ${
                            darkMode ? 'text-green-400' : 'text-green-600'
                          }`}
                        >
                          {networkData.network}
                        </td>
                      )}
                      <td className="p-4">
                        <div className="flex items-center space-x-2">
                          <img src={fee.logo} alt={fee.exchange} className="w-6 h-6 rounded" />
                          <span>{fee.exchange}</span>
                        </div>
                      </td>
                      <td className="p-4 font-medium">{fee.fee} {selectedCoin}</td>
                      <td className="p-4">{fee.minAmount} {selectedCoin}</td>
                      <td className="p-4">
                        {feeIndex === 0 && (
                          <div className="flex items-center space-x-1">
                            <svg className="w-4 h-4 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M5 13l4 4L19 7" />
                            </svg>
                            <span className={`text-sm ${
                              darkMode ? 'text-green-400' : 'text-green-600'
                            }`}>
                              Lowest
                            </span>
                          </div>
                        )}
                      </td>
                    </tr>
                  ))}
                </React.Fragment>
              ))}
            </tbody>
          </table>
        </div>
      </div>

      {!premiumUser && (
        <div className={`p-6 rounded-xl ${darkMode ? 'bg-gray-800 border border-gray-700' : 'bg-blue-50 border border-blue-200'}`}>
          <div className="flex flex-col md:flex-row md:items-center md:justify-between">
            <div className="mb-4 md:mb-0">
              <h3 className={`text-lg font-semibold ${darkMode ? 'text-white' : 'text-gray-900'}`}>
                Unlock Premium Features
              </h3>
              <p className={darkMode ? 'text-gray-400' : 'text-gray-600'}>
                Get access to historical fee charts, price alerts, and advanced analytics
              </p>
            </div>
            <button
              onClick={() => setShowLoginModal(true)}
              className={`px-6 py-3 rounded-lg font-semibold ${
                darkMode
                  ? 'bg-blue-600 hover:bg-blue-700 text-white'
                  : 'bg-blue-500 hover:bg-blue-600 text-white'
              }`}
            >
              Upgrade to Premium
            </button>
          </div>
        </div>
      )}
    </div>
  );

  const LoginModal = () => {
    const [isLogin, setIsLogin] = useState(true);

    const handleSubmit = (e) => {
      e.preventDefault();
      setUser({ name: 'John Doe', email: email });
      setPremiumUser(email === 'premium@example.com');
      setShowLoginModal(false);
      setEmail('');
      setPassword('');
    };

    if (!showLoginModal) return null;

    return (
      <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
        <div className={`w-full max-w-md rounded-2xl p-6 ${
          darkMode ? 'bg-gray-800' : 'bg-white'
        }`}>
          <div className="flex justify-between items-center mb-6">
            <h2 className={`text-2xl font-bold ${darkMode ? 'text-white' : 'text-gray-900'}`}>
              {isLogin ? 'Sign In' : 'Create Account'}
            </h2>
            <button
              onClick={() => setShowLoginModal(false)}
              className={`p-2 rounded-lg ${darkMode ? 'hover:bg-gray-700' : 'hover:bg-gray-100'}`}
            >
              ×
            </button>
          </div>

          <form onSubmit={handleSubmit}>
            <div className="space-y-4">
              <div>
                <label className={`block text-sm font-medium mb-2 ${darkMode ? 'text-gray-300' : 'text-gray-700'}`}>
                  Email
                </label>
                <input
                  type="email"
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  className={`w-full p-3 rounded-lg border ${
                    darkMode 
                      ? 'bg-gray-700 border-gray-600 text-white' 
                      : 'bg-white border-gray-300 text-gray-900'
                  }`}
                  required
                />
              </div>

              <div>
                <label className={`block text-sm font-medium mb-2 ${darkMode ? 'text-gray-300' : 'text-gray-700'}`}>
                  Password
                </label>
                <input
                  type="password"
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  className={`w-full p-3 rounded-lg border ${
                    darkMode 
                      ? 'bg-gray-700 border-gray-600 text-white' 
                      : 'bg-white border-gray-300 text-gray-900'
                  }`}
                  required
                />
              </div>

              <button
                type="submit"
                className={`w-full py-3 px-4 rounded-lg font-semibold ${
                  darkMode
                    ? 'bg-blue-600 hover:bg-blue-700 text-white'
                    : 'bg-blue-500 hover:bg-blue-600 text-white'
                }`}
              >
                {isLogin ? 'Sign In' : 'Create Account'}
              </button>
            </div>
          </form>

          <div className="mt-4 text-center">
            <button
              onClick={() => setIsLogin(!isLogin)}
              className={`text-sm ${
                darkMode ? 'text-blue-400 hover:text-blue-300' : 'text-blue-600 hover:text-blue-800'
              }`}
            >
              {isLogin ? "Don't have an account? Sign up" : "Already have an account? Sign in"}
            </button>
          </div>
        </div>
      </div>
    );
  };

  return (
    <div className={darkMode ? 'bg-gray-900 text-white' : 'bg-gray-50 text-gray-900'}>
      <header className={`sticky top-0 z-40 ${
        darkMode ? 'bg-gray-900 border-b border-gray-800' : 'bg-white border-b border-gray-200'
      }`}>
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex items-center justify-between h-16">
            <div className="flex items-center space-x-8">
              <h1 
                onClick={() => {
                  setActivePage('home');
                  setSelectedExchange(null);
                }}
                className="text-2xl font-bold cursor-pointer"
              >
                <span className={darkMode ? 'text-blue-400' : 'text-blue-600'}>Crypto</span>
                <span>Fee</span>
                <span className={darkMode ? 'text-green-400' : 'text-green-600'}>Compare</span>
              </h1>
              
              <nav className="hidden md:flex space-x-8">
                <button
                  onClick={() => setActivePage('home')}
                  className={`font-medium transition-colors ${
                    activePage === 'home'
                      ? darkMode ? 'text-blue-400' : 'text-blue-600'
                      : darkMode ? 'text-gray-400 hover:text-gray-300' : 'text-gray-600 hover:text-gray-900'
                  }`}
                >
                  Exchanges
                </button>
                <button
                  onClick={() => setActivePage('wallet')}
                  className={`font-medium transition-colors ${
                    activePage === 'wallet'
                      ? darkMode ? 'text-blue-400' : 'text-blue-600'
                      : darkMode ? 'text-gray-400 hover:text-gray-300' : 'text-gray-600 hover:text-gray-900'
                  }`}
                >
                  Wallet Fees
                </button>
              </nav>
            </div>

            <div className="flex items-center space-x-4">
              {user ? (
                <div className="flex items-center space-x-4">
                  <div className={`text-sm ${
                    darkMode ? 'text-gray-300' : 'text-gray-700'
                  }`}>
                    {premiumUser && (
                      <span className="inline-flex items-center px-2 py-1 rounded-full text-xs font-medium bg-purple-100 text-purple-800 mr-2">
                        Premium
                      </span>
                    )}
                    {user.name}
                  </div>
                  <button
                    onClick={handleLogout}
                    className={`px-3 py-1 rounded text-sm ${
                      darkMode 
                        ? 'bg-gray-800 hover:bg-gray-700' 
                        : 'bg-gray-200 hover:bg-gray-300'
                    }`}
                  >
                    Logout
                  </button>
                </div>
              ) : (
                <button
                  onClick={() => setShowLoginModal(true)}
                  className={`px-4 py-2 rounded-lg font-medium ${
                    darkMode 
                      ? 'bg-gray-800 hover:bg-gray-700 text-white' 
                      : 'bg-white hover:bg-gray-100 text-gray-900 shadow-sm'
                  }`}
                >
                  Sign In
                </button>
              )}
              
              <button
                onClick={toggleDarkMode}
                className={`p-2 rounded-lg ${
                  darkMode 
                    ? 'bg-gray-800 hover:bg-gray-700' 
                    : 'bg-gray-200 hover:bg-gray-300'
                }`}
              >
                {darkMode ? (
                  <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
                  </svg>
                ) : (
                  <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
                  </svg>
                )}
              </button>
            </div>
          </div>
        </div>
      </header>

      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        {activePage === 'home' && <HomePage />}
        {activePage === 'exchange' && <ExchangeDetailPage />}
        {activePage === 'wallet' && <WalletComparisonPage />}
      </main>

      <footer className={`mt-16 py-8 ${
        darkMode ? 'bg-gray-800 border-t border-gray-700' : 'bg-white border-t border-gray-200'
      }`}>
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="grid grid-cols-1 md:grid-cols-4 gap-8">
            <div className="col-span-1 md:col-span-2">
              <h3 className={`text-xl font-bold mb-4 ${
                darkMode ? 'text-white' : 'text-gray-900'
              }`}>
                <span className={darkMode ? 'text-blue-400' : 'text-blue-600'}>Crypto</span>
                <span>Fee</span>
                <span className={darkMode ? 'text-green-400' : 'text-green-600'}>Compare</span>
              </h3>
              <p className={`mb-4 ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>
                The most comprehensive crypto exchange fee comparison platform. 
                Make informed decisions about where to trade and withdraw your crypto.
              </p>
              <div className="flex space-x-4">
                <div className={`px-3 py-1 rounded-full text-sm ${
                  darkMode ? 'bg-gray-700 text-gray-300' : 'bg-gray-200 text-gray-800'
                }`}>
                  Updated Daily
                </div>
                <div className={`px-3 py-1 rounded-full text-sm ${
                  darkMode ? 'bg-gray-700 text-gray-300' : 'bg-gray-200 text-gray-800'
                }`}>
                  Real-time Data
                </div>
              </div>
            </div>
            
            <div>
              <h4 className={`font-semibold mb-4 ${darkMode ? 'text-white' : 'text-gray-900'}`}>Quick Links</h4>
              <ul className={`space-y-2 ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>
                <li>
                  <button 
                    onClick={() => setActivePage('home')}
                    className="hover:text-blue-500 transition-colors"
                  >
                    Exchanges
                  </button>
                </li>
                <li>
                  <button 
                    onClick={() => setActivePage('wallet')}
                    className="hover:text-blue-500 transition-colors"
                  >
                    Wallet Fees
                  </button>
                </li>
                <li>
                  <button 
                    onClick={() => setShowLoginModal(true)}
                    className="hover:text-blue-500 transition-colors"
                  >
                    Premium
                  </button>
                </li>
              </ul>
            </div>
            
            <div>
              <h4 className={`font-semibold mb-4 ${darkMode ? 'text-white' : 'text-gray-900'}`}>Monetization</h4>
              <ul className={`space-y-2 ${darkMode ? 'text-gray-400' : 'text-gray-600'}`}>
                <li>✔️ Affiliate Links</li>
                <li>✔️ Premium Memberships</li>
                <li>✔️ Crypto Advertising</li>
                <li>✔️ API Access</li>
              </ul>
            </div>
          </div>
          
          <div className={`mt-8 pt-8 border-t ${
            darkMode ? 'border-gray-700 text-gray-400' : 'border-gray-200 text-gray-600'
          } text-sm text-center`}>
            <p>© 2024 CryptoFeeCompare. All data is for informational purposes only.</p>
            <p className="mt-2">
              This site contains affiliate links. We may earn commissions on sign-ups.
            </p>
          </div>
        </div>
      </footer>

      <LoginModal />
    </div>
  );
};

export default App;
