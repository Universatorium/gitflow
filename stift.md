Hallo Andy
let counts = [0, 0, 0, 0, 0, 0];
let countEls = [
  blablabla
  document.getElementById("anz1"),
  document.getElementById("anz2"),
  document.getElementById("anz3"),
  document.getElementById("anz4"),
  document.getElementById("anz5"),
  document.getElementById("anz6"),hrthtrhrhrth
];
let productPrices = [];

function anzPlus(index) {
  counts[index] = counts[index] + 1;
  countEls[index].innerText = "Anzahl: " + counts[index];
  updateGesamtpreis();
}

function anzMinus(index) {
  if (counts[index] > 0) {
    counts[index] = counts[index] - 1;
    countEls[index].innerText = "Anzahl: " + counts[index];
    updateGesamtpreis();
  }
}

function fetchProducts(numberOfProducts) {
  const requests = [];

  for (let i = 1; i <= numberOfProducts; i++) {
    requests.push(fetch(`https://dummyjson.com/products/${i}`).then((res) => res.json()));
  }

  return Promise.all(requests);
}

function updateBox(product, index) {
  const box = document.querySelector(`.box:nth-child(${index})`);
  const title = box.querySelector(".title strong");
  const price = box.querySelector(".price em");
  const image = box.querySelector(".bild");

  title.innerText = product.title;
  price.innerText = product.price.toFixed(2) + "€";
  image.src = product.thumbnail;
  productPrices[index - 1] = product.price;
}

function updateGesamtpreis() {
  let gesamt = 0;

  for (let i = 0; i < productPrices.length; i++) {
    gesamt += counts[i] * productPrices[i];
  }

  let gesamtEl = document.getElementById("gesamtpreis");
  gesamtEl.innerText = gesamt.toFixed(2) + "€";
}

fetchProducts(6)
  .then((products) => {
    products.forEach((product, index) => {
      updateBox(product, index + 1);
    });
  })
  .then(() => {
    for (let i = 0; i < 6; i++) {
      document.getElementById(`plus-btn${i + 1}`).addEventListener("click", () => anzPlus(i));
      document.getElementById(`minus-btn${i + 1}`).addEventListener("click", () => anzMinus(i));
    }

    updateGesamtpreis();
  })
  .catch((error) => {
    console.error("Fehler beim Abrufen der Produkte:", error);
  });
