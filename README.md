import React, { useState } from "react";
import { motion } from "framer-motion";

// BeautyGlow — Tienda Virtual (Vista Previa Completa)
// Funcionalidades incluidas: navbar con categorías, páginas, más productos, QR dinámico,
// varias imágenes por producto, carrito con cantidades, modal de detalle, footer y logo placeholder.

const initialProducts = [
  {
    id: 1,
    name: "Máscara LED Terapia Facial",
    priceNumber: 249.0,
    price: "S/ 249",
    images: [
      "/images/products/led-mask-1.jpg",
      "/images/products/led-mask-2.jpg",
      "/images/products/led-mask-3.jpg",
    ],
    desc:
      "Máscara LED con 7 colores de terapia, mejora textura y luminosidad. Uso 10 min/día.",
    category: "Beauty Tech",
  },
  {
    id: 2,
    name: "Serum Vitamina C 30ml",
    priceNumber: 89.0,
    price: "S/ 89",
    images: [
      "/images/products/serum-c-1.jpg",
      "/images/products/serum-c-2.jpg",
    ],
    desc: "Serum estabilizado con vitamina C para iluminar la piel y reducir manchas.",
    category: "Skincare",
  },
  {
    id: 3,
    name: "Set Brochas Pro (7 piezas)",
    priceNumber: 129.0,
    price: "S/ 129",
    images: [
      "/images/products/brushes-1.jpg",
      "/images/products/brushes-2.jpg",
    ],
    desc: "Set profesional de brochas suaves, ideal para todo tipo de maquillaje.",
    category: "Maquillaje",
  },
  {
    id: 4,
    name: "Parches de Colágeno x30",
    priceNumber: 39.0,
    price: "S/ 39",
    images: ["/images/products/eye-patches.jpg"],
    desc: "Parches hidratantes para contorno de ojos con colágeno y péptidos.",
    category: "Skincare",
  },
  {
    id: 5,
    name: "Pads Desmaquillantes Reutilizables (x7)",
    priceNumber: 29.0,
    price: "S/ 29",
    images: ["/images/products/reusable-pads.jpg"],
    desc: "Ecológicos, lavables y suaves para piel sensible. Pack x7 con bolsa de algodón.",
    category: "Accesorios",
  },
  {
    id: 6,
    name: "Espejo LED Vanity con 3 niveles",
    priceNumber: 169.0,
    price: "S/ 169",
    images: ["/images/products/mirror-led.jpg"],
    desc: "Espejo con luz regulable, enchufe USB y base giratoria. Ideal para maquillaje.",
    category: "Accesorios",
  },
  {
    id: 7,
    name: "Gua-Sha + Roller Set",
    priceNumber: 59.0,
    price: "S/ 59",
    images: ["/images/products/gua-sha.jpg"],
    desc: "Kit de masaje facial para drenaje linfático y mejorar firmeza.",
    category: "Skincare",
  },
  {
    id: 8,
    name: "Cepillo Masaje Capilar",
    priceNumber: 45.0,
    price: "S/ 45",
    images: ["/images/products/scalp-brush.jpg"],
    desc: "Estimula circulación y ayuda al cuidado del cuero cabelludo.",
    category: "Accesorios",
  },
];

export default function TiendaPreview() {
  const [view, setView] = useState("home"); // home | categories | payments | cart
  const [products] = useState(initialProducts);
  const [selectedProduct, setSelectedProduct] = useState(null);
  const [cart, setCart] = useState([]);
  const [activeImageIndex, setActiveImageIndex] = useState(0);

  const WHATSAPP = "51" + "977975805"; // número provisto por el usuario (format: 51977975805)

  function addToCart(product, qty = 1) {
    setCart((prev) => {
      const found = prev.find((p) => p.id === product.id);
      if (found) {
        return prev.map((p) => (p.id === product.id ? { ...p, qty: p.qty + qty } : p));
      }
      return [...prev, { ...product, qty }];
    });
  }

  function removeFromCart(productId) {
    setCart((prev) => prev.filter((p) => p.id !== productId));
  }

  function updateQty(productId, qty) {
    if (qty <= 0) return removeFromCart(productId);
    setCart((prev) => prev.map((p) => (p.id === productId ? { ...p, qty } : p)));
  }

  const cartTotal = cart.reduce((s, p) => s + p.priceNumber * p.qty, 0);

  function openProduct(product) {
    setSelectedProduct(product);
    setActiveImageIndex(0);
    setView("product");
  }

  function buyByWhatsApp(product, qty = 1) {
    const text = `Hola,%20estoy%20interesado(a)%20en%20${encodeURIComponent(
      product.name
    )}%20(x${qty})`;
    const url = `https://wa.me/${WHATSAPP}?text=${text}`;
    window.open(url, "_blank");
  }

  // QR dinámico usando Google Chart API (solo texto con total o producto)
  function qrForAmount(amount) {
    const payload = `Pagar%20a%20BeautyGlow%20Store%20-%20Monto:%20S/${amount.toFixed(2)}`;
    return `https://chart.googleapis.com/chart?chs=300x300&cht=qr&chl=${encodeURIComponent(
      payload
    )}`;
  }

  return (
    <div className="min-h-screen bg-gray-50 text-gray-800">
      {/* NAVBAR */}
      <header className="bg-white shadow sticky top-0 z-40">
        <div className="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between gap-6">
          <div className="flex items-center gap-3">
            <img src="/images/logo-luxury.png" alt="Logo" className="w-12 h-12 object-contain" />
            <div>
              <div className="text-xl font-extrabold">BeautyGlow Store</div>
              <div className="text-xs text-gray-500">Belleza y accesorios premium</div>
            </div>
          </div>

          <nav className="hidden md:flex gap-6 items-center">
            <button onClick={() => setView("home")} className="hover:text-pink-600">Inicio</button>
            <button onClick={() => setView("categories")} className="hover:text-pink-600">Categorías</button>
            <button onClick={() => setView("products")} className="hover:text-pink-600">Productos</button>
            <button onClick={() => setView("payments")} className="hover:text-pink-600">Métodos de Pago</button>
            <button onClick={() => setView("contact")} className="hover:text-pink-600">Contacto</button>
          </nav>

          <div className="flex items-center gap-4">
            <div className="hidden md:block text-sm text-gray-600">🛒 {cart.length} items</div>
            <button onClick={() => setView("cart")} className="bg-pink-600 text-white px-4 py-2 rounded-lg">Ver carrito</button>
          </div>
        </div>
      </header>

      {/* HERO */}
      {view === "home" && (
        <main className="max-w-6xl mx-auto px-6 py-10">
          <section className="grid grid-cols-1 md:grid-cols-2 gap-8 items-center bg-gradient-to-r from-pink-400 to-rose-600 text-white rounded-2xl p-8 shadow-lg">
            <div>
              <h1 className="text-4xl md:text-5xl font-extrabold mb-4">Brilla con estilo — BeautyGlow</h1>
              <p className="mb-6 text-lg opacity-90">Productos seleccionados para cuidar tu piel y realzar tu belleza natural. Envíos a todo el Perú y pago con Yape/Plin o WhatsApp.</p>
              <div className="flex gap-4">
                <button onClick={() => setView("products")} className="bg-white text-pink-600 px-6 py-3 rounded-lg font-semibold">Ver catálogo</button>
                <a href={`https://wa.me/${WHATSAPP}?text=Hola,%20quiero%20más%20info%20sobre%20sus%20productos`} className="bg-green-500 text-white px-6 py-3 rounded-lg font-semibold">Atención por WhatsApp</a>
              </div>
            </div>

            <motion.img
              initial={{ opacity: 0, scale: 0.9 }}
              animate={{ opacity: 1, scale: 1 }}
              transition={{ duration: 0.8 }}
              src="/images/banners/hero-beauty.jpg"
              alt="Hero Beauty"
              className="w-full rounded-xl object-cover h-80 md:h-96"
            />
          </section>

          {/* CATEGORÍAS INICIO */}
          <section className="mt-10">
            <h2 className="text-2xl font-bold mb-4">Categorías</h2>
            <div className="grid grid-cols-2 md:grid-cols-4 gap-6">
              {[...new Set(products.map((p) => p.category))].map((cat) => (
                <div key={cat} className="bg-white rounded-xl shadow p-6 text-center">
                  <img
                    src="/images/categories/category-placeholder.jpg"
                    alt={cat}
                    className="w-full h-32 object-cover rounded-md mb-3"
                  />
                  <h3 className="font-semibold">{cat}</h3>
                  <p className="text-sm text-gray-500 mt-1">
                    Explora los mejores productos de {cat}
                  </p>
                </div>
              ))}
            </div>
          </section>

          {/* Destacados */}
          <section className="mt-10">
            <h2 className="text-2xl font-bold mb-4">Productos destacados</h2>
            <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
              {products.slice(0, 6).map((p) => (
                <article key={p.id} className="bg-white rounded-xl shadow p-4 flex flex-col">
                  <img src={p.images[0]} alt={p.name} className="w-full h-44 object-cover rounded-md mb-3" />
                  <div className="flex-1">
                    <h3 className="font-semibold">{p.name}</h3>
                    <p className="text-pink-600 font-bold mt-2">{p.price}</p>
                  </div>
                  <div className="mt-3 flex gap-2">
                    <button onClick={() => openProduct(p)} className="flex-1 bg-white border border-pink-200 text-pink-600 py-2 rounded-lg">Ver</button>
                    <button onClick={() => addToCart(p)} className="flex-1 bg-pink-600 text-white py-2 rounded-lg">Añadir</button>
                  </div>
                </article>
              ))}
            </div>
          </section>

        </main>
      )}

      {/* CATEGORIES */}
      {view === "categories" && (
        <main className="max-w-6xl mx-auto px-6 py-10">
          <h2 className="text-2xl font-bold mb-6">Categorías</h2>
          <div className="grid grid-cols-2 md:grid-cols-4 gap-6">
            {[...new Set(products.map((p) => p.category))].map((cat) => (
              <div key={cat} className="bg-white rounded-xl shadow p-6 text-center">
                <img src="/images/categories/category-placeholder.jpg" alt={cat} className="w-full h-32 object-cover rounded-md mb-3" />
                <h3 className="font-semibold">{cat}</h3>
                <p className="text-sm text-gray-500 mt-1">Explora los mejores productos de {cat}</p>
              </div>
            ))}
          </div>
        </main>
      )}

      {/* PRODUCTS LIST */}
      {view === "products" && (
        <main className="max-w-6xl mx-auto px-6 py-10">
          <h2 className="text-2xl font-bold mb-6">Todos los productos</h2>
          <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
            {products.map((p) => (
              <article key={p.id} className="bg-white rounded-xl shadow p-4 flex flex-col">
                <img src={p.images[0]} alt={p.name} className="w-full h-44 object-cover rounded-md mb-3" />
                <div className="flex-1">
                  <h3 className="font-semibold">{p.name}</h3>
                  <p className="text-pink-600 font-bold mt-2">{p.price}</p>
                </div>
                <div className="mt-3 flex gap-2">
                  <button onClick={() => openProduct(p)} className="flex-1 bg-white border border-pink-200 text-pink-600 py-2 rounded-lg">Ver</button>
                  <button onClick={() => addToCart(p)} className="flex-1 bg-pink-600 text-white py-2 rounded-lg">Añadir</button>
                </div>
              </article>
            ))}
          </div>
        </main>
      )}

      {/* PAYMENTS */}
      {view === "payments" && (
        <main className="max-w-4xl mx-auto px-6 py-10">
          <h2 className="text-2xl font-bold mb-4">Métodos de pago</h2>
          <div className="bg-white rounded-xl shadow p-6">
            <ul className="space-y-3 text-gray-700">$1</ul>

            {/* FORMULARIO TARJETA */}
            <div className="mt-6 p-4 border rounded-xl bg-gray-50">
              <h4 className="font-semibold mb-3">Pagar con tarjeta de crédito / débito</h4>
              <form className="grid grid-cols-1 gap-4" onSubmit={(e)=>{e.preventDefault(); const form=e.target; const name=form[0].value.trim(); const num=form[1].value.trim(); const exp=form[2].value.trim(); const cvv=form[3].value.trim(); if(!name||!num||!exp||!cvv){alert('Completa todos los campos'); return;} if(!/^\d{16}$/.test(num)){alert('Número de tarjeta inválido'); return;} if(!/^\d{2}\/\d{2}$/.test(exp)){alert('Formato de fecha inválido (MM/AA)'); return;} if(!/^\d{3}$/.test(cvv)){alert('CVV inválido'); return;} alert('Pago procesado (demo)');}}>
                <input type="text" placeholder="Nombre en la tarjeta" className="border p-2 rounded" />
                <input type="text" placeholder="Número de tarjeta" className="border p-2 rounded" />
                <div className="grid grid-cols-2 gap-4">
                  <input type="text" placeholder="MM/AA" className="border p-2 rounded" />
                  <input type="text" placeholder="CVV" className="border p-2 rounded" />
                </div>
                <button type="submit" className="bg-pink-600 text-white py-2 rounded-lg">Procesar pago</button>
                <p className="text-xs text-gray-500 mt-2">* Modo demostración — integración real próximamente</p>
              </form>
            </div>

            <div className="mt-6">
              <h4 className="font-semibold mb-2">QR de pago (dinámico para el total del carrito)</h4>
              {cart.length === 0 ? (
                <p className="text-gray-500">El carrito está vacío — agrega productos para generar un QR con el total.</p>
              ) : (
                <div className="flex flex-col items-center gap-4">
                  <img src={qrForAmount(cartTotal)} alt="QR de pago" className="w-48 h-48" />
                  <div>Escanea con Yape o Plin para pagar S/ {cartTotal.toFixed(2)}</div>
                </div>
              )}
            </div>
          </div>
        </main>
      )}

      {/* CONTACT */}
      {view === "contact" && (
        <main className="max-w-4xl mx-auto px-6 py-10">
          <h2 className="text-2xl font-bold mb-4">Contacto</h2>
          <div className="bg-white rounded-xl shadow p-6">
            <p className="mb-4">Atención al cliente por WhatsApp: <a className="text-green-600 font-semibold" href={`https://wa.me/${WHATSAPP}`}>+51 {"977975805"}</a></p>
            <p className="text-gray-600">Horario de atención: Lun-Vie 9:00 - 18:00</p>
          </div>
        </main>
      )}

      {/* CART */}
      {view === "cart" && (
        <main className="max-w-4xl mx-auto px-6 py-10">
          <h2 className="text-2xl font-bold mb-4">Tu carrito</h2>
          <div className="bg-white rounded-xl shadow p-6 space-y-4">
            {cart.length === 0 ? (
              <p className="text-gray-500">Tu carrito está vacío. Añade productos desde el catálogo.</p>
            ) : (
              <>
                {cart.map((item) => (
                  <div key={item.id} className="flex items-center gap-4">
                    <img src={item.images[0]} alt={item.name} className="w-20 h-20 object-cover rounded-md" />
                    <div className="flex-1">
                      <div className="font-semibold">{item.name}</div>
                      <div className="text-pink-600 font-bold">S/ {item.priceNumber.toFixed(2)}</div>
                      <div className="mt-2 flex items-center gap-2">
                        <button onClick={() => updateQty(item.id, item.qty - 1)} className="px-2 py-1 bg-gray-100 rounded">-</button>
                        <div>{item.qty}</div>
                        <button onClick={() => updateQty(item.id, item.qty + 1)} className="px-2 py-1 bg-gray-100 rounded">+</button>
                      </div>
                    </div>
                    <div>
                      <button onClick={() => removeFromCart(item.id)} className="text-red-500">Eliminar</button>
                    </div>
                  </div>
                ))}

                <div className="mt-4 text-right">
                  <div className="text-gray-600">Subtotal: S/ {cartTotal.toFixed(2)}</div>
                  <div className="mt-2 flex gap-2 justify-end">
                    <button onClick={() => setView("payments")} className="bg-pink-600 text-white px-4 py-2 rounded">Pagar</button>
                    <a href={`https://wa.me/${WHATSAPP}?text=Hola,%20quiero%20pagar%20mi%20pedido%20de%20S/${cartTotal.toFixed(2)}`} className="bg-green-500 text-white px-4 py-2 rounded">Pagar por WhatsApp</a>
                  </div>
                </div>
              </>
            )}
          </div>
        </main>
      )}

      {/* PRODUCT DETAIL VIEW (modal-like page) */}
      {view === "product" && selectedProduct && (
        <main className="max-w-4xl mx-auto px-6 py-10">
          <div className="bg-white rounded-xl shadow p-6 grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <img src={selectedProduct.images[activeImageIndex]} alt={selectedProduct.name} className="w-full h-72 object-cover rounded-md mb-3" />
              <div className="flex gap-2">
                {selectedProduct.images.map((img, i) => (
                  <img key={i} src={img} alt={`thumb-${i}`} onClick={() => setActiveImageIndex(i)} className={`w-16 h-16 object-cover rounded cursor-pointer ${i===activeImageIndex? 'ring-2 ring-pink-600':''}`} />
                ))}
              </div>
            </div>

            <div>
              <h2 className="text-2xl font-bold">{selectedProduct.name}</h2>
              <div className="text-pink-600 font-bold text-2xl mt-2">{selectedProduct.price}</div>
              <p className="mt-4 text-gray-700">{selectedProduct.desc}</p>

              <div className="mt-6 flex gap-3">
                <button onClick={() => addToCart(selectedProduct)} className="bg-pink-600 text-white px-4 py-3 rounded-lg">Añadir al carrito</button>
                <button onClick={() => buyByWhatsApp(selectedProduct)} className="bg-green-500 text-white px-4 py-3 rounded-lg">Comprar por WhatsApp</button>
              </div>

            </div>
          </div>
        </main>
      )}

      {/* FOOTER */}
      <footer className="mt-12 bg-gray-900 text-gray-200 py-8">
        <div className="max-w-6xl mx-auto px-6 grid grid-cols-1 md:grid-cols-3 gap-6">
          <div>
            <div className="text-2xl font-extrabold">BeautyGlow Store</div>
            <div className="text-sm text-gray-400 mt-2">Belleza y accesorios premium — Envíos a todo el Perú</div>
          </div>

          <div>
            <div className="font-semibold mb-2">Soporte</div>
            <div className="text-sm text-gray-400">WhatsApp: <a className="text-green-400" href={`https://wa.me/${WHATSAPP}`}>+51 977 975 805</a></div>
            <div className="text-sm text-gray-400 mt-1">Email: soporte@beautyglow.store</div>
          </div>

          <div>
            <div className="font-semibold mb-2">Métodos de pago</div>
            <div className="text-sm text-gray-400">Yape / Plin / Transferencia / Tarjeta (próx.)</div>
          </div>
        </div>

        <div className="max-w-6xl mx-auto px-6 text-center text-sm text-gray-500 mt-6">© 2025 BeautyGlow Store — Todos los derechos reservados</div>
      </footer>
    </div>
  );
}
