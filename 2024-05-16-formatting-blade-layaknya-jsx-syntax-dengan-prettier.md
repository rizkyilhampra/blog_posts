---
author: rip
categories:
  - engineering
date: 2024-05-16 00:25:05 +0800
tags:
  - blade
  - laravel
  - tipsandtrick
title: Formatting Blade layaknya JSX Syntax dengan Prettier
---
Kali ini akan bahas sebuah blade formatter plugin dari Prettier yang akan memformat kode blade layaknya sebuah kode jsx. Jika anda pernah mencoba React.js, tentu tidak asing dengan yang namanya jsx file atau Javascript XML. JSX memungkinkan kita untuk menulis kode javascript dan markup html didalamnya. Ini serupa dengan blade yang mana kita juga bisa menulis kode javascript, markup html, php dan bahkan sql beserta '*directive*'-nya. Di javascript ecosystem kita tau ada formatter populer yaitu Prettier, umumnya orang akan menggunakan Prettier dalam hal memformat kode javascript khususnya jsx. Fungsi formatter disini adalah untuk merapikan indentasi dan sebagainya, sehingga mudah di baca atau di lihat. Ekstensi jsx telah mendapatkan support dari prettier secara langsung (bukan sebagai plugin dengan parser tambahan) sehingga kita bisa langsung menggunakannya ke dalam kode jsx kita. 

Penggunaan formatter prettier dalam memformat kode jsx, akan terdapat beberapa perubahan, contohnya ketika beberapa atribut dalam satu element terlalu banyak, maka ia akan dibuatkan garis baru setelah start tag dengan tambahan identasi. Berikut adalah contohnya. 

```jsx
return (
	<div
		id="app-container"
		className="app-wrapper"
		style={{
		    backgroundColor: '#f5f5f5',
		    padding: '20px',
		    borderRadius: '8px',
		    boxShadow: '0 2px 4px rgba(0, 0, 0, 0.1)'
		}}
		data-role="main-content"
		aria-label="Application Container"
		tabIndex="0"
		onKeyDown={(e) => handleKeyPress(e)}
	>
	</div>
)
```

Umumnya formatter untuk kode markup html tidak melakukan hal demikian. Dimana formatter untuk kode markup html, ia akan menggabungkan semua attribute pada suatu element, tidak perduli mau itu panjang atau pendek, pada satu garis yang sama dengan start tag element tersebut. Berikut adalah contohnya.

```html
<div id="app-container" class="app-wrapper" style=" backgroundColor: '#f5f5f5'; padding: '20px'; borderRadius: '8px'; boxShadow: '0 2px 4px rgba(0, 0, 0, 0.1)';" data-role="main-content" aria-label="Application Container" tabIndex="0" onKeyDown="(e) => handleKeyPress(e)"></div>
```