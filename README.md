<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Menu Restoran</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #fafafa;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      margin-bottom: 30px;
    }
    #menu-container {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
      gap: 20px;
    }
    .menu-item {
      border: 1px solid #ddd;
      border-radius: 10px;
      background: #fff;
      padding: 15px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      text-align: center;
      transition: transform 0.2s;
    }
    .menu-item:hover {
      transform: scale(1.03);
    }
    .menu-item img {
      max-width: 100%;
      border-radius: 8px;
      height: 150px;
      object-fit: cover;
      margin-bottom: 10px;
    }
    .menu-item h3 {
      margin: 10px 0 5px;
    }
    .menu-item p {
      margin: 5px 0;
    }
    .price {
      font-weight: bold;
      color: #e63946;
    }
  </style>
</head>
<body>
  <h1>🍝 Daftar Menu Restoran</h1>
  <div id="menu-container">Loading menu...</div>

  <!-- Supabase -->
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <script>
    const { createClient } = supabase
    // 🔑 Ganti dengan project kamu
    const supabaseUrl = "https://xxx.supabase.co"      // SUPABASE_URL
    const supabaseKey = "eyJhbGciOi..."                // SUPABASE_ANON_KEY
    const db = createClient(supabaseUrl, supabaseKey)

    async function loadMenus() {
      let { data, error } = await db.from("menus").select("*")
      if (error) {
        console.error(error)
        document.getElementById("menu-container").innerHTML = "❌ Gagal memuat menu!"
        return
      }

      let container = document.getElementById("menu-container")
      container.innerHTML = ""
      data.forEach(menu => {
        container.innerHTML += `
          <div class="menu-item">
            <img src="${menu.img || 'https://via.placeholder.com/200'}" alt="${menu.name}" />
            <h3>${menu.name}</h3>
            <p>${menu.desk || ''}</p>
            <p><em>Kategori: ${menu.cat}</em></p>
            <p class="price">Rp ${menu.price.toLocaleString()}</p>
          </div>
        `
      })
    }

    loadMenus()
  </script>
</body>
</html>
