# ====================== BOOK MARKET BOT (Single File) ======================
# Fayl nomi: main.py

import asyncio
import sqlite3
import json
from datetime import datetime
from aiogram import Bot, Dispatcher, F
from aiogram.filters import Command
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton, WebAppInfo
from flask import Flask, render_template_string, request, jsonify
from flask_cors import CORS
import os
from dotenv import load_dotenv

load_dotenv()

# ====================== CONFIG ======================
BOT_TOKEN = os.getenv("BOT_TOKEN")
ADMIN_ID = int(os.getenv("ADMIN_ID", 0))
WEBAPP_URL = os.getenv("WEBAPP_URL", "http://127.0.0.1:5000")

bot = Bot(token=BOT_TOKEN)
dp = Dispatcher()

# ====================== DATABASE ======================
def init_db():
    conn = sqlite3.connect('database.db')
    conn.executescript('''
        CREATE TABLE IF NOT EXISTS books (
            id INTEGER PRIMARY KEY,
            title TEXT, author TEXT, price REAL,
            image TEXT, video TEXT, description TEXT,
            category TEXT, rating REAL DEFAULT 0,
            likes INTEGER DEFAULT 0
        );
        CREATE TABLE IF NOT EXISTS reviews (
            id INTEGER PRIMARY KEY,
            book_id INTEGER, user_id INTEGER,
            text TEXT, rating INTEGER, date TEXT
        );
    ''')
    conn.commit()
    conn.close()

def get_db():
    conn = sqlite3.connect('database.db')
    conn.row_factory = sqlite3.Row
    return conn

# ====================== FLASK WEB APP ======================
app = Flask(__name__)
CORS(app)

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kitob Market</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
    body { font-family: 'Inter', sans-serif; }
    .card { transition: all 0.3s ease; }
    .card:hover { transform: translateY(-8px); box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1); }
    .carousel { scroll-behavior: smooth; }
    .theme-light { --bg: #f8fafc; --text: #1e2937; --accent: #6366f1; }
    .theme-dark { --bg: #0f172a; --text: #e2e8f0; --accent: #818cf8; }
    .theme-relax { --bg: #f0f9f6; --text: #1f2937; --accent: #14b8a6; }
    .theme-vibrant { --bg: #fdf4ff; --text: #581c87; --accent: #c026d3; }
  </style>
</head>
<body class="theme-light bg-[var(--bg)] text-[var(--text)] min-h-screen">
  <div class="max-w-7xl mx-auto p-4 pb-24">
    <!-- Header -->
    <div class="flex justify-between items-center mb-6 sticky top-0 bg-[var(--bg)] z-50 py-3">
      <h1 class="text-3xl font-bold" style="background: linear-gradient(90deg, var(--accent), #a855f7); -webkit-background-clip: text; color: transparent;">
        📚 Kitob Market
      </h1>
      <select id="theme-select" onchange="setTheme(this.value)" class="bg-white dark:bg-slate-800 px-4 py-2 rounded-2xl border border-gray-300 focus:outline-none">
        <option value="light">Yorug'</option>
        <option value="dark">Qorong'i</option>
        <option value="relax">Dam olish</option>
        <option value="vibrant">Yorqin</option>
      </select>
    </div>

    <!-- Carousel -->
    <div id="carousel" class="relative h-72 md:h-80 mb-8 overflow-x-auto flex gap-6 snap-x carousel"></div>

    <!-- Categories -->
    <div class="flex gap-3 overflow-x-auto pb-4 mb-6" id="categories"></div>

    <!-- Books Grid -->
    <div id="books-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-6"></div>
  </div>

  <!-- Book Modal -->
  <div id="modal" class="fixed inset-0 bg-black/70 hidden flex items-center justify-center z-50">
    <div class="bg-[var(--bg)] rounded-3xl max-w-lg w-full mx-4 max-h-[90vh] overflow-auto">
      <div id="modal-content" class="p-6"></div>
    </div>
  </div>

  <script>
    const tg = window.Telegram.WebApp;
    tg.expand();
    let cart = [];
    let booksData = [];

    function setTheme(theme) {
      document.body.className = `theme-${theme}`;
      localStorage.setItem('theme', theme);
    }

    async function loadBooks() {
      const res = await fetch('/api/books');
      booksData = await res.json();
      renderBooks(booksData);
      renderCarousel(booksData);
      renderCategories(booksData);
    }

    function renderCarousel(books) {
      const container = document.getElementById('carousel');
      container.innerHTML = books.slice(0, 8).map(book => `
        <div onclick="showBook(${book.id})" class="min-w-[260px] snap-center cursor-pointer">
          <img src="${book.image}" class="w-full h-64 object-cover rounded-3xl shadow-xl" alt="${book.title}">
          <p class="mt-3 font-semibold text-lg">${book.title}</p>
          <p class="text-sm text-gray-500">${book.author} • ${book.price.toLocaleString()} so'm</p>
        </div>
      `).join('');
    }

    function renderBooks(books) {
      const grid = document.getElementById('books-grid');
      grid.innerHTML = books.map(book => `
        <div onclick="showBook(${book.id})" class="card bg-white dark:bg-slate-800 rounded-3xl overflow-hidden shadow-lg cursor-pointer">
          <img src="${book.image}" class="w-full h-52 object-cover">
          <div class="p-4">
            <h3 class="font-semibold text-lg leading-tight">${book.title}</h3>
            <p class="text-sm text-gray-500">${book.author}</p>
            <div class="flex justify-between items-center mt-4">
              <span class="font-bold text-xl">${book.price.toLocaleString()} so'm</span>
              <button onclick="event.stopImmediatePropagation(); toggleLike(${book.id})" class="text-2xl">❤️</button>
            </div>
          </div>
        </div>
      `).join('');
    }

    function renderCategories(books) {
      const cats = [...new Set(books.map(b => b.category))];
      const container = document.getElementById('categories');
      container.innerHTML = `<button onclick="filterCategory('all')" class="category-btn active px-5 py-2 rounded-2xl whitespace-nowrap">Barchasi</button>` +
        cats.map(cat => `
          <button onclick="filterCategory('${cat}')" class="category-btn px-5 py-2 bg-white dark:bg-slate-800 rounded-2xl whitespace-nowrap">${cat}</button>
        `).join('');
    }

    function filterCategory(cat) {
      if (cat === 'all') renderBooks(booksData);
      else renderBooks(booksData.filter(b => b.category === cat));
    }

    async function showBook(id) {
      const res = await fetch(`/api/book/${id}`);
      const data = await res.json();
      const book = data.book;

      document.getElementById('modal-content').innerHTML = `
        <button onclick="closeModal()" class="float-right text-3xl">✕</button>
        <img src="${book.image}" class="w-full h-80 object-cover rounded-3xl">
        ${book.video ? `<iframe class="w-full h-64 mt-4 rounded-2xl" src="${book.video}" allowfullscreen></iframe>` : ''}
        <h2 class="text-2xl font-bold mt-6">${book.title}</h2>
        <p class="text-lg text-gray-500">${book.author}</p>
        <p class="text-3xl font-bold mt-2">${book.price.toLocaleString()} so'm</p>
        <p class="mt-6 text-gray-600">${book.description}</p>
        <button onclick="addToCart(${JSON.stringify(book)})" class="w-full mt-8 py-4 bg-[var(--accent)] text-white text-xl font-semibold rounded-3xl">
          Savatchaga qo'shish
        </button>
      `;
      document.getElementById('modal').classList.remove('hidden');
    }

    function closeModal() {
      document.getElementById('modal').classList.add('hidden');
    }

    function addToCart(book) {
      cart.push(book);
      tg.MainButton.setText(`Savatda (${cart.length}) • Xarid qilish`);
      tg.MainButton.show();
    }

    function toggleLike(id) {
      alert("❤️ Yoqdi! (Backendda saqlash mumkin)");
      event.stopImmediatePropagation();
    }

    // Telegram Main Button
    tg.MainButton.onClick(() => {
      alert(`Rahmat! Siz ${cart.length} ta kitob sotib oldingiz. To'lov integratsiyasini qo'shish mumkin.`);
      cart = [];
      tg.MainButton.hide();
    });

    // Init
    window.onload = () => {
      loadBooks();
      const savedTheme = localStorage.getItem('theme') || 'light';
      document.getElementById('theme-select').value = savedTheme;
      setTheme(savedTheme);
    };
  </script>
</body>
</html>
"""

@app.route('/')
def index():
    return render_template_string(HTML_TEMPLATE)

@app.route('/api/books')
def get_books():
    conn = get_db()
    books = conn.execute("SELECT * FROM books ORDER BY likes DESC").fetchall()
    return jsonify([dict(book) for book in books])

@app.route('/api/book/<int:book_id>')
def get_book(book_id):
    conn = get_db()
    book = conn.execute("SELECT * FROM books WHERE id=?", (book_id,)).fetchone()
    if not book:
        return jsonify({"error": "Not found"}), 404
    reviews = conn.execute("SELECT * FROM reviews WHERE book_id=?", (book_id,)).fetchall()
    return jsonify({"book": dict(book), "reviews": [dict(r) for r in reviews]})

@app.route('/api/admin/add_book', methods=['POST'])
def add_book():
    if int(request.headers.get('X-User-Id', 0)) != ADMIN_ID:
        return jsonify({"error": "Access denied"}), 403
    data = request.json
    conn = get_db()
    conn.execute("""
        INSERT INTO books (title, author, price, image, video, description, category)
        VALUES (?,?,?,?,?,?,?)
    """, (data['title'], data['author'], data['price'], data['image'],
          data.get('video'), data['description'], data['category']))
    conn.commit()
    return jsonify({"success": True, "id": conn.lastrowid})

# ====================== TELEGRAM BOT ======================
@dp.message(Command("start"))
async def start(message):
    kb = InlineKeyboardMarkup(inline_keyboard=[[
        InlineKeyboardButton(
            text="🛒 Kitoblar do'konini ochish",
            web_app=WebAppInfo(url=WEBAPP_URL)
        )
    ]])
    await message.answer(
        "Assalomu alaykum! 📖\n\n"
        "<b>Kitob Market</b> ga xush kelibsiz.\n"
        "Eng yaxshi kitoblarni toping va xarid qiling.",
        reply_markup=kb,
        parse_mode="HTML"
    )

# ====================== RUN ======================
async def run_bot():
    await dp.start_polling(bot)

if __name__ == "__main__":
    init_db()
    print("🚀 Kitob Market ishga tushdi!")
    print("Admin ID:", ADMIN_ID)
    
    # Test uchun bir nechta kitob qo'shish
    conn = get_db()
    if conn.execute("SELECT COUNT(*) FROM books").fetchone()[0] == 0:
        sample_books = [
            ("Atomic Habits", "James Clear", 89000, "https://picsum.photos/id/20/800/600", "", "Hayotni o'zgartiruvchi kitob", "O'z-o'zini rivojlantirish"),
            ("Rich Dad Poor Dad", "Robert Kiyosaki", 65000, "https://picsum.photos/id/201/800/600", "", "Moliyaviy savodxonlik", "Moliya"),
            ("The Psychology of Money", "Morgan Housel", 75000, "https://picsum.photos/id/133/800/600", "", "Pul va psixologiya", "Moliya"),
        ]
        for book in sample_books:
            conn.execute("INSERT INTO books (title,author,price,image,video,description,category) VALUES (?,?,?,?,?,?,?)", book)
        conn.commit()
    conn.close()

    from threading import Thread
    Thread(target=lambda: app.run(host='0.0.0.0', port=5000, debug=False)).start()
    asyncio.run(run_bot())
