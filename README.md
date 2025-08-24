index.html
import React, { useState, useEffect } from 'react';
import { Search, MapPin, ChefHat, Filter, Eye, MessageCircle, PlusCircle, TrendingUp, Home, Edit3, BarChart3 } from 'lucide-react';

const KyodoCookingSite = () => {
  const [articles, setArticles] = useState([]);
  const [currentPage, setCurrentPage] = useState('home');
  const [searchTerm, setSearchTerm] = useState('');
  const [filterPrefecture, setFilterPrefecture] = useState('');
  const [filterCategory, setFilterCategory] = useState('');
  const [selectedArticle, setSelectedArticle] = useState(null);
  const [newComment, setNewComment] = useState('');

  const prefectures = [
    '北海道', '青森県', '岩手県', '宮城県', '秋田県', '山形県', '福島県', '茨城県', '栃木県', '群馬県',
    '埼玉県', '千葉県', '東京都', '神奈川県', '新潟県', '富山県', '石川県', '福井県', '山梨県', '長野県',
    '岐阜県', '静岡県', '愛知県', '三重県', '滋賀県', '京都府', '大阪府', '兵庫県', '奈良県', '和歌山県',
    '鳥取県', '島根県', '岡山県', '広島県', '山口県', '徳島県', '香川県', '愛媛県', '高知県', '福岡県',
    '佐賀県', '長崎県', '熊本県', '大分県', '宮崎県', '鹿児島県', '沖縄県'
  ];

  const categories = ['調味料', '野菜', '魚', 'その他'];

  const [newArticle, setNewArticle] = useState({
    title: '',
    content: '',
    prefecture: '',
    localDish: '',
    category: '',
    author: ''
  });

  const filteredArticles = articles.filter(article => {
    const matchesSearch = article.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
                         article.content.toLowerCase().includes(searchTerm.toLowerCase()) ||
                         article.localDish.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesPrefecture = filterPrefecture === '' || article.prefecture === filterPrefecture;
    const matchesCategory = filterCategory === '' || article.category === filterCategory;
    return matchesSearch && matchesPrefecture && matchesCategory;
  });

  const handleSubmitArticle = () => {
    if (newArticle.title && newArticle.content && newArticle.prefecture && newArticle.localDish && newArticle.category && newArticle.author) {
      const article = {
        id: Date.now(),
        ...newArticle,
        date: new Date().toLocaleDateString('ja-JP'),
        views: 0,
        comments: []
      };
      setArticles([...articles, article]);
      setNewArticle({
        title: '',
        content: '',
        prefecture: '',
        localDish: '',
        category: '',
        author: ''
      });
      setCurrentPage('home');
      alert('記事が投稿されました！');
    } else {
      alert('すべての項目を入力してください。');
    }
  };

  const handleArticleClick = (article) => {
    const updatedArticles = articles.map(a => 
      a.id === article.id ? { ...a, views: a.views + 1 } : a
    );
    setArticles(updatedArticles);
    setSelectedArticle({ ...article, views: article.views + 1 });
    setCurrentPage('article');
  };

  const handleAddComment = () => {
    if (newComment.trim() && selectedArticle) {
      const comment = {
        id: Date.now(),
        text: newComment,
        date: new Date().toLocaleDateString('ja-JP'),
        time: new Date().toLocaleTimeString('ja-JP', { hour: '2-digit', minute: '2-digit' })
      };
      const updatedArticles = articles.map(article =>
        article.id === selectedArticle.id
          ? { ...article, comments: [...article.comments, comment] }
          : article
      );
      setArticles(updatedArticles);
      setSelectedArticle({ ...selectedArticle, comments: [...selectedArticle.comments, comment] });
      setNewComment('');
    }
  };

  const popularArticles = [...articles].sort((a, b) => b.views - a.views).slice(0, 5);
  const totalViews = articles.reduce((sum, article) => sum + article.views, 0);
  const totalComments = articles.reduce((sum, article) => sum + article.comments.length, 0);

  return (
    <div className="min-h-screen bg-green-50">
      {/* ヘッダー */}
      <header className="bg-green-100 shadow-md">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex justify-between items-center py-4">
            <h1 
              className="text-2xl sm:text-3xl font-bold text-green-800 cursor-pointer flex items-center"
              onClick={() => setCurrentPage('home')}
            >
              <ChefHat className="mr-2" />
              科学的郷土料理研究所
            </h1>
            <nav className="flex space-x-2 sm:space-x-4">
              <button
                onClick={() => setCurrentPage('home')}
                className={`flex items-center px-3 py-2 rounded-md ${currentPage === 'home' ? 'bg-green-200 text-green-800' : 'text-green-700 hover:bg-green-200'}`}
              >
                <Home className="w-4 h-4 mr-1" />
                <span className="hidden sm:inline">ホーム</span>
              </button>
              <button
                onClick={() => setCurrentPage('post')}
                className={`flex items-center px-3 py-2 rounded-md ${currentPage === 'post' ? 'bg-green-200 text-green-800' : 'text-green-700 hover:bg-green-200'}`}
              >
                <Edit3 className="w-4 h-4 mr-1" />
                <span className="hidden sm:inline">記事投稿</span>
              </button>
              <button
                onClick={() => setCurrentPage('analytics')}
                className={`flex items-center px-3 py-2 rounded-md ${currentPage === 'analytics' ? 'bg-green-200 text-green-800' : 'text-green-700 hover:bg-green-200'}`}
              >
                <BarChart3 className="w-4 h-4 mr-1" />
                <span className="hidden sm:inline">分析</span>
              </button>
            </nav>
          </div>
        </div>
      </header>

      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        {/* ホームページ */}
        {currentPage === 'home' && (
          <div>
            <div className="text-center mb-8">
              <h2 className="text-3xl font-bold text-green-800 mb-4">科学的根拠に基づく郷土料理の世界</h2>
              <p className="text-green-600 text-lg">日本各地の郷土料理を科学的な視点から探求し、その魅力を発見しましょう</p>
            </div>

            {/* 検索・フィルター */}
            <div className="bg-white rounded-lg shadow-md p-6 mb-8">
              <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
                <div className="relative">
                  <Search className="absolute left-3 top-3 w-4 h-4 text-green-400" />
                  <input
                    type="text"
                    placeholder="記事を検索..."
                    className="w-full pl-10 pr-4 py-2 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500 focus:border-transparent"
                    value={searchTerm}
                    onChange={(e) => setSearchTerm(e.target.value)}
                  />
                </div>
                <select
                  className="px-4 py-2 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500"
                  value={filterPrefecture}
                  onChange={(e) => setFilterPrefecture(e.target.value)}
                >
                  <option value="">すべての県</option>
                  {prefectures.map(pref => (
                    <option key={pref} value={pref}>{pref}</option>
                  ))}
                </select>
                <select
                  className="px-4 py-2 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500"
                  value={filterCategory}
                  onChange={(e) => setFilterCategory(e.target.value)}
                >
                  <option value="">すべてのカテゴリ</option>
                  {categories.map(cat => (
                    <option key={cat} value={cat}>{cat}</option>
                  ))}
                </select>
                <button
                  onClick={() => {
                    setSearchTerm('');
                    setFilterPrefecture('');
                    setFilterCategory('');
                  }}
                  className="px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600 flex items-center justify-center"
                >
                  <Filter className="w-4 h-4 mr-2" />
                  リセット
                </button>
              </div>
            </div>

            {/* 記事一覧 */}
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
              {filteredArticles.length === 0 ? (
                <div className="col-span-full text-center py-12">
                  <p className="text-green-600 text-lg">記事がありません。最初の記事を投稿してみませんか？</p>
                </div>
              ) : (
                filteredArticles.map(article => (
                  <div key={article.id} className="bg-white rounded-lg shadow-md p-6 hover:shadow-lg transition-shadow cursor-pointer">
                    <div onClick={() => handleArticleClick(article)}>
                      <h3 className="text-xl font-semibold text-green-800 mb-3 hover:text-green-600">{article.title}</h3>
                      <div className="flex items-center text-sm text-green-600 mb-2">
                        <MapPin className="w-4 h-4 mr-1" />
                        <span>{article.prefecture}</span>
                        <span className="mx-2">•</span>
                        <span>{article.localDish}</span>
                      </div>
                      <div className="flex items-center text-sm text-green-600 mb-3">
                        <span className="bg-green-100 px-2 py-1 rounded">{article.category}</span>
                      </div>
                      <p className="text-gray-600 mb-4 line-clamp-3">{article.content.substring(0, 120)}...</p>
                      <div className="flex justify-between items-center text-sm text-green-500">
                        <span>by {article.author}</span>
                        <div className="flex items-center space-x-3">
                          <div className="flex items-center">
                            <Eye className="w-4 h-4 mr-1" />
                            <span>{article.views}</span>
                          </div>
                          <div className="flex items-center">
                            <MessageCircle className="w-4 h-4 mr-1" />
                            <span>{article.comments.length}</span>
                          </div>
                          <span>{article.date}</span>
                        </div>
                      </div>
                    </div>
                  </div>
                ))
              )}
            </div>
          </div>
        )}

        {/* 記事詳細ページ */}
        {currentPage === 'article' && selectedArticle && (
          <div className="max-w-4xl mx-auto">
            <button
              onClick={() => setCurrentPage('home')}
              className="mb-6 text-green-600 hover:text-green-800 flex items-center"
            >
              ← ホームに戻る
            </button>
            <article className="bg-white rounded-lg shadow-md p-8">
              <h1 className="text-3xl font-bold text-green-800 mb-4">{selectedArticle.title}</h1>
              <div className="flex flex-wrap items-center text-sm text-green-600 mb-6">
                <div className="flex items-center mr-4 mb-2">
                  <MapPin className="w-4 h-4 mr-1" />
                  <span>{selectedArticle.prefecture}</span>
                </div>
                <div className="flex items-center mr-4 mb-2">
                  <ChefHat className="w-4 h-4 mr-1" />
                  <span>{selectedArticle.localDish}</span>
                </div>
                <span className="bg-green-100 px-3 py-1 rounded mr-4 mb-2">{selectedArticle.category}</span>
                <div className="flex items-center mr-4 mb-2">
                  <Eye className="w-4 h-4 mr-1" />
                  <span>{selectedArticle.views} views</span>
                </div>
              </div>
              <div className="text-sm text-green-600 mb-6">
                by {selectedArticle.author} • {selectedArticle.date}
              </div>
              <div className="prose max-w-none mb-8">
                <p className="text-gray-700 leading-relaxed whitespace-pre-wrap">{selectedArticle.content}</p>
              </div>

              {/* コメント欄 */}
              <div className="border-t pt-8">
                <h3 className="text-xl font-semibold text-green-800 mb-4 flex items-center">
                  <MessageCircle className="w-5 h-5 mr-2" />
                  コメント ({selectedArticle.comments.length})
                </h3>
                
                {/* コメント投稿 */}
                <div className="mb-6">
                  <textarea
                    className="w-full p-4 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500 focus:border-transparent resize-none"
                    rows="3"
                    placeholder="コメントを書く..."
                    value={newComment}
                    onChange={(e) => setNewComment(e.target.value)}
                  />
                  <button
                    onClick={handleAddComment}
                    className="mt-2 px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600"
                  >
                    コメントを投稿
                  </button>
                </div>

                {/* コメント一覧 */}
                <div className="space-y-4">
                  {selectedArticle.comments.length === 0 ? (
                    <p className="text-gray-500">まだコメントがありません。最初のコメントを投稿してみませんか？</p>
                  ) : (
                    selectedArticle.comments.map(comment => (
                      <div key={comment.id} className="bg-green-50 p-4 rounded-md">
                        <p className="text-gray-700 mb-2">{comment.text}</p>
                        <p className="text-sm text-green-600">{comment.date} {comment.time}</p>
                      </div>
                    ))
                  )}
                </div>
              </div>
            </article>
          </div>
        )}

        {/* 記事投稿ページ */}
        {currentPage === 'post' && (
          <div className="max-w-2xl mx-auto">
            <h2 className="text-3xl font-bold text-green-800 mb-8">新しい記事を投稿</h2>
            <div className="bg-white rounded-lg shadow-md p-8">
              <div className="space-y-6">
                <div>
                  <label className="block text-sm font-medium text-green-700 mb-2">記事タイトル</label>
                  <input
                    type="text"
                    className="w-full p-3 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500"
                    placeholder="記事のタイトルを入力..."
                    value={newArticle.title}
                    onChange={(e) => setNewArticle({...newArticle, title: e.target.value})}
                  />
                </div>

                <div>
                  <label className="block text-sm font-medium text-green-700 mb-2">著者名</label>
                  <input
                    type="text"
                    className="w-full p-3 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500"
                    placeholder="あなたの名前を入力..."
                    value={newArticle.author}
                    onChange={(e) => setNewArticle({...newArticle, author: e.target.value})}
                  />
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                  <div>
                    <label className="block text-sm font-medium text-green-700 mb-2">都道府県</label>
                    <select
                      className="w-full p-3 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500"
                      value={newArticle.prefecture}
                      onChange={(e) => setNewArticle({...newArticle, prefecture: e.target.value})}
                    >
                      <option value="">都道府県を選択</option>
                      {prefectures.map(pref => (
                        <option key={pref} value={pref}>{pref}</option>
                      ))}
                    </select>
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-green-700 mb-2">カテゴリ</label>
                    <select
                      className="w-full p-3 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500"
                      value={newArticle.category}
                      onChange={(e) => setNewArticle({...newArticle, category: e.target.value})}
                    >
                      <option value="">カテゴリを選択</option>
                      {categories.map(cat => (
                        <option key={cat} value={cat}>{cat}</option>
                      ))}
                    </select>
                  </div>
                </div>

                <div>
                  <label className="block text-sm font-medium text-green-700 mb-2">郷土料理名</label>
                  <input
                    type="text"
                    className="w-full p-3 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500"
                    placeholder="郷土料理の名前を入力..."
                    value={newArticle.localDish}
                    onChange={(e) => setNewArticle({...newArticle, localDish: e.target.value})}
                  />
                </div>

                <div>
                  <label className="block text-sm font-medium text-green-700 mb-2">記事内容</label>
                  <textarea
                    className="w-full p-3 border border-green-300 rounded-md focus:ring-2 focus:ring-green-500 resize-none"
                    rows="10"
                    placeholder="記事の内容を入力してください。科学的根拠や研究データを含めて詳しく書いてください..."
                    value={newArticle.content}
                    onChange={(e) => setNewArticle({...newArticle, content: e.target.value})}
                  />
                </div>

                <button
                  onClick={handleSubmitArticle}
                  className="w-full py-3 bg-green-500 text-white rounded-md hover:bg-green-600 font-medium flex items-center justify-center"
                >
                  <PlusCircle className="w-5 h-5 mr-2" />
                  記事を投稿する
                </button>
              </div>
            </div>
          </div>
        )}

        {/* 分析ページ */}
        {currentPage === 'analytics' && (
          <div>
            <h2 className="text-3xl font-bold text-green-800 mb-8">記事分析ダッシュボード</h2>
            
            {/* 統計情報 */}
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
              <div className="bg-white rounded-lg shadow-md p-6">
                <div className="flex items-center">
                  <div className="p-3 bg-green-100 rounded-lg">
                    <Eye className="w-6 h-6 text-green-600" />
                  </div>
                  <div className="ml-4">
                    <p className="text-sm font-medium text-gray-600">総閲覧数</p>
                    <p className="text-2xl font-bold text-green-800">{totalViews}</p>
                  </div>
                </div>
              </div>
              <div className="bg-white rounded-lg shadow-md p-6">
                <div className="flex items-center">
                  <div className="p-3 bg-green-100 rounded-lg">
                    <MessageCircle className="w-6 h-6 text-green-600" />
                  </div>
                  <div className="ml-4">
                    <p className="text-sm font-medium text-gray-600">総コメント数</p>
                    <p className="text-2xl font-bold text-green-800">{totalComments}</p>
                  </div>
                </div>
              </div>
              <div className="bg-white rounded-lg shadow-md p-6">
                <div className="flex items-center">
                  <div className="p-3 bg-green-100 rounded-lg">
                    <ChefHat className="w-6 h-6 text-green-600" />
                  </div>
                  <div className="ml-4">
                    <p className="text-sm font-medium text-gray-600">総記事数</p>
                    <p className="text-2xl font-bold text-green-800">{articles.length}</p>
                  </div>
                </div>
              </div>
            </div>

            {/* 人気記事ランキング */}
            <div className="bg-white rounded-lg shadow-md p-6">
              <h3 className="text-xl font-semibold text-green-800 mb-4 flex items-center">
                <TrendingUp className="w-5 h-5 mr-2" />
                人気記事ランキング
              </h3>
              {popularArticles.length === 0 ? (
                <p className="text-gray-500">まだ記事がありません。</p>
              ) : (
                <div className="space-y-4">
                  {popularArticles.map((article, index) => (
                    <div key={article.id} className="flex items-center p-4 bg-green-50 rounded-lg">
                      <div className="flex-shrink-0 w-8 h-8 bg-green-500 text-white rounded-full flex items-center justify-center font-bold">
                        {index + 1}
                      </div>
                      <div className="ml-4 flex-1">
                        <h4 className="font-medium text-green-800">{article.title}</h4>
                        <p className="text-sm text-green-600">{article.prefecture} • {article.localDish}</p>
                      </div>
                      <div className="text-right">
                        <div className="flex items-center text-green-600 mb-1">
                          <Eye className="w-4 h-4 mr-1" />
                          <span className="font-medium">{article.views}</span>
                        </div>
                        <div className="flex items-center text-green-600">
                          <MessageCircle className="w-4 h-4 mr-1" />
                          <span>{article.comments.length}</span>
                        </div>
                      </div>
                    </div>
                  ))}
                </div>
              )}
            </div>
          </div>
        )}
      </main>
    </div>
  );
};

export default KyodoCookingSite;
