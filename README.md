
# One Piece Infinite Scroll Gallery

> **Este projeto foi incorporado ao [One Piece Codex](https://hugohendrix.github.io/one-piece-codex/)** ([código](https://github.com/HugoHendrix/one-piece-codex)), que reúne esta landing page, a galeria com rolagem infinita e a lista de personagens em React. Este repositório fica como registro do original.

Uma galeria com rolagem infinita inspirada em **One Piece**, proporcionando uma experiência visual contínua e interativa.

## 🖥️ Tecnologias Utilizadas
- HTML
- CSS
- JavaScript

## 📂 Estrutura de Arquivos
- `index.html`: Estrutura da página.
- `style.css`: Estilos da galeria e da página.
- `index.js`: Lógica de rolagem infinita.


## 📋 Funcionalidades
- **Rolagem infinita horizontal:** 
  - Os itens se repetem continuamente, criando uma sensação de fluidez.
- **Pause ao passar o mouse:** 
  - A rolagem para automaticamente quando o cursor está sobre a galeria.
- **Adaptação à tela:** 
  - O layout se ajusta ao redimensionar a janela do navegador.



### Explicação do Código:

Aqui está uma versão mais detalhada e clara da explicação, agora com o código relevante incluído:

---

### **Explicação do Código**

#### **HTML**
O HTML fornece a estrutura básica para a página e organiza os elementos da galeria.

**Estrutura da página:**
   - Estrutura central da página:
     ```html
     <div id="photo-gallery-container">
         <div id="photo-gallery">
             <!-- Imagens com links -->
             <a class="photo-link" href="URL_IMAGEM_LARGA">
                 <img class="photo" src="URL_THUMBNAIL" alt="Descrição da Imagem">
             </a>
             <!-- Outras imagens seguem a mesma estrutura -->
         </div>
     </div>
     ```
   - A `div` `#photo-gallery` hospeda os itens (imagens com links).

---

#### **CSS**
**Galeria e imagens:**
   - Configuração básica:
     ```css
     #photo-gallery-container {
         white-space: nowrap;
         width: 100vw;
         position: fixed;
         top: 50%;
         transform: translateY(-50%);
         padding: 3vh 0;
     }

     #photo-gallery {
         display: inline-block;
         width: max-content;
     }
     ```

   - Estilo das imagens:
     ```css
     .photo-link {
         display: inline-block;
         width: 33vh;
         height: 33vh;
         margin: 0 3vh;
         border-radius: 5px;
         box-shadow: 0 12.5px 12.5px rgba(0, 0, 0, .15);
         overflow: hidden;
         transition: transform 0.3s;
     }

     .photo-link:hover {
         transform: scale(1.15);
     }

     .photo {
         width: 100%;
         height: 100%;
         object-fit: cover;
     }
     ```

---

#### **JavaScript**

1. **Função principal:**
   - Cria a rolagem contínua e replica os itens.
     ```javascript
     function initInfiniteCarousel(selector, speed) {
         const carousel = document.querySelector(selector);
         if (!carousel) return;

         const originalItems = Array.from(carousel.querySelectorAll('a'));
         let totalScroll = 0;
         let singleCycleWidth = 0;

         // Clonar itens até preencher a tela
         function fillCarousel() {
             singleCycleWidth = originalItems.reduce((acc, item) => {
                 return acc + item.offsetWidth + parseFloat(getComputedStyle(item).marginRight);
             }, 0);

             const numClones = Math.ceil(window.innerWidth / singleCycleWidth) * 2;
             for (let i = 0; i < numClones; i++) {
                 originalItems.forEach(item => {
                     const clone = item.cloneNode(true);
                     clone.classList.add('clone');
                     carousel.appendChild(clone);
                 });
             }
         }

         // Animar a rolagem
         function animateScroll() {
             totalScroll += speed;
             if (totalScroll >= singleCycleWidth) totalScroll = 0;
             carousel.style.transform = `translateX(-${totalScroll}px)`;
             requestAnimationFrame(animateScroll);
         }

         fillCarousel();
         animateScroll();
     }
     ```

2. **Configuração:**
   - Inicializa a rolagem com velocidade controlada:
     ```javascript
     initInfiniteCarousel('#photo-gallery', 1);
     ```

3. **Debounce (opcional):**
   - Evita execuções excessivas em eventos como `resize`:
     ```javascript
     function debounce(func, wait) {
         let timeout;
         return (...args) => {
             clearTimeout(timeout);
             timeout = setTimeout(() => func.apply(this, args), wait);
         };
     }
     ```

---

