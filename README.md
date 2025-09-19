const masterKey = '$2a$10$azU7cjtH/qwMSKoty0Jm1uSaWYNnY8qPJFIQTEFNu4/XkVsZVZWqG';
const binId = '68cc142142be1c4f08a35142';
const url = `https://api.jsonbin.io/v3/b/${binId}/latest`;

fetch(url, {
  method: 'GET',
  headers: {
    'X-Master-Key': masterKey
  }
})
.then(response => {
  if (response.ok) {
    return response.json();
  } else {
    throw new Error('Hata: ' + response.statusText);
  }
})
.then(data => {
  console.log('✅ Bağlantı başarılı! Veri:', data);
})
.catch(error => {
  console.error('❌ Bağlantı hatası:', error);
});
