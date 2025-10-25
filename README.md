# Bekhruzbe.github.io
// Akvarium containerini tanlaymiz
const aquarium = document.getElementById('aquarium');

// Baliq rasmlari (transparent PNG)
const fishImages = [
  'https://upload.wikimedia.org/wikipedia/commons/7/77/Goldfish3.png', // transparent PNG
  'https://upload.wikimedia.org/wikipedia/commons/0/0b/Goldfish2.png'
];

// Baliqlar soni
const numFish = 10;

// Har bir baliqni yaratamiz
for (let i = 0; i < numFish; i++) {
  const fish = document.createElement('img');
  fish.src = fishImages[Math.floor(Math.random() * fishImages.length)];
  fish.classList.add('fish');

  // Baliqning boshlang'ich top va left pozitsiyalari
  fish.style.top = Math.random() * 80 + '%'; // 0% - 80% vertical
  fish.style.left = Math.random() * window.innerWidth + 'px'; // tasodifiy horizontal

  // Baliqni akvariumga qo'shamiz
  aquarium.appendChild(fish);

  // Tasodifiy tezlik va yo'nalish
  let speed = 0.5 + Math.random() * 1.5; // px/frame
  let direction = Math.random() < 0.5 ? 1 : -1; // 1 -> o'ng, -1 -> chap

  // Agar chapga suzsa, scaleX(-1) bilan o'girish
  if (direction === -1) fish.style.transform = 'scaleX(-1)';

  // Tasodifiy y-eng tebranish qo'shish (tabiiyroq harakat)
  let amplitude = 20 + Math.random() * 30; // suzish tebranishi pikselda
  let frequency = 0.01 + Math.random() * 0.02; // tebranish tezligi
  let startTop = parseFloat(fish.style.top);

  // Baliqni animatsiya qilish
  function moveFish(frame = 0) {
    let currentLeft = parseFloat(fish.style.left);
    currentLeft += speed * direction;

    // Agar ekran chetida bo'lsa, yo'nalishni o'zgartirish
    if (currentLeft > window.innerWidth) {
      direction = -1;
      fish.style.transform = 'scaleX(-1)';
    }
    if (currentLeft < -100) {
      direction = 1;
      fish.style.transform = 'scaleX(1)';
    }

    // Vertikal tebranish qo'shamiz
    let yOffset = amplitude * Math.sin(frame * frequency);
    fish.style.top = startTop + yOffset + 'px';

    // Yangilangan left
    fish.style.left = currentLeft + 'px';

    // Keyingi frame
    requestAnimationFrame(() => moveFish(frame + 1));
  }

  // Animatsiyani boshlash
  moveFish();
}

// Agar oynani resize qilinsa, baliq pozitsiyalarini moslash
window.addEventListener('resize', () => {
  const fishes = document.querySelectorAll('.fish');
  fishes.forEach(fish => {
    let left = parseFloat(fish.style.left);
    if (left > window.innerWidth) fish.style.left = window.innerWidth - 60 + 'px';
  });
});
