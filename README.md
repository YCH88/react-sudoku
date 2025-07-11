# 🎯 Ekstra Kolay Sudoku

React ile geliştirilmiş temel bir Sudoku oyunu. 9x9 Sudoku tahtası üzerinde kullanıcıların sayı girişi yapmasına, doğruluğunu kontrol etmesine ve yeni oyun başlatmasına olanak tanır. Başlangıç seviyesi için uygundur.

## 🚀 Özellikler

- 🎲 Rastgele oluşturulmuş başlangıç Sudoku tahtası (Kolay seviye - 35 ipucu).
- 🔢 Sayı seçme ve tahtaya yerleştirme.
- ❌ Hatalı hücreleri gösterme.
- ✅ Doğrulama sistemi ve başarı kutlaması.
- 🧠 Temel çözüm algoritması ile otomatik Sudoku üretimi.
- 🖱️ Kullanıcı dostu, tıklanabilir arayüz.

---

## 🛠️ Kullanılan Teknolojiler

- **React (JavaScript)**: UI bileşenleri ve kullanıcı etkileşimleri için.
- **CSS**: Stillerin yönetimi için.
- **useState, useEffect**: Durum yönetimi ve yan etkiler için.
- **Backtracking Algoritması**: Sudoku çözücü ve üretici algoritmalar.
- **Fonksiyonel Bileşenler**: Uygulama genelinde fonksiyonel bileşen yapısı kullanıldı.


---

## 🧩 Oyun Dinamiği

- Başlangıçta otomatik olarak rastgele oluşturulmuş bir Sudoku tahtası yüklenir.
- Kullanıcı, numpad üzerinden sayı seçerek boş hücrelere yerleştirebilir.
- İstenirse `Doğrula` butonuyla doğru ve yanlış girişler vurgulanır.
- `Yeni Oyun` ile her seferinde farklı bir puzzle başlatılır.
- Tüm hücreler doğruysa tebrik mesajı görüntülenir 🎉

---

## 🧑‍💻 Kullandığım Yapılar

Bu projede, **React**'ın çeşitli özelliklerini kullanarak dinamik ve etkileşimli bir Sudoku uygulaması geliştirdim. Aşağıda kullandığım ana yapılar ve araçlar açıklanmıştır:

### 1. **useState** ve **useEffect**
- **useState**: React bileşenlerindeki durumu yönetmek için kullanıldı. Kullanıcı tarafından girilen sayılar, seçilen hücre ve hata kontrolü gibi veriler bu state'ler ile kontrol ediliyor.
  - Örnek:
    ```javascript
    const [board, setBoard] = useState(null); // Kullanıcı tarafından güncellenen tablo
    const [selectedNumber, setSelectedNumber] = useState(null); // Seçilen sayı
    ```

- **useEffect**: İlk yükleme sırasında Sudoku tahtasını oluşturmak için kullanılan bu hook, sadece bileşen ilk render edildiğinde çalışır ve başlangıç verilerini ayarlar.
  - Örnek:
    ```javascript
    useEffect(() => {
      const generated = generateSudoku();
      setInitialBoard(generated);
      setBoard(generated.map(row => [...row])); // Kopyasını alıyoruz ki orijinal bozulmasın
    }, []); // Boş bağımlılık dizisi ile sadece ilk renderda çalışır
    ```

### 2. **useCallback** ve **useMemo**
Bu projede, `useCallback` ve `useMemo` gibi performans optimizasyonları için React hook'ları kullanılabilir. (Henüz projede bunlar görünmese de ilerleyen aşamalarda kullanılabilir.)

- **useCallback**: Sadece belirli bir durumda yeniden hesaplanan fonksiyonları optimize etmek için.
- **useMemo**: Hesaplama maliyeti yüksek olan değerlerin yeniden hesaplanmaması için kullanılır.

### 3. **useTimer (Custom Hook)**  
Projenizde zamanlayıcı fonksiyonu mevcut değil, ancak bu özelliği geliştirmek isterseniz, bir `useTimer` hook'u oluşturmak iyi bir fikir olabilir. Bu, oyuncunun ne kadar sürede oyunu bitirdiğini gösteren bir özellik olabilir.

### 4. **Event Handling**
- Hücre tıklama olayları ve sayı seçme işlemleri, **onClick** gibi event handler'ları ile yönetiliyor.
  - Örnek: 
    ```javascript
    const handleCellClick = (rowIndex, colIndex, isPreFilled) => {
      if (isPreFilled) return;
      setSelectedCell({ row: rowIndex, col: colIndex });
      if (selectedNumber !== null) {
        const newBoard = board.map((row) => row.slice());
        newBoard[rowIndex][colIndex] = selectedNumber;
        setBoard(newBoard);
      }
    };
    ```

### 5. **Conditional Rendering**
- **Yazılımsal mantık ve koşullar**: Hücrelerin seçilmesi, yanlış hücrelerin vurgulanması ve başarı durumunda kutlama mesajı gibi durumlar, `if` koşulları ile kontrol edilip, kullanıcıya uygun şekilde render ediliyor.
  - Örnek:
    ```javascript
    {showCelebration && (
      <div className="celebration-overlay">
        <div className="celebration-text">🎉 Bravo! Sudoku Tamamlandı! 🎉</div>
      </div>
    )}
    ```

### 6. **Component-Based Architecture**
Projede **component-based** yapı kullanılmıştır. Her bir bileşen, belirli bir sorumluluğa sahiptir ve UI'yi daha modüler hale getirmek için bölünmüştür:
- **SudokuTable.js**: Sudoku tahtasının görsel temsilini yönetir.
- **NumberPad.js**: Kullanıcının sayıları seçebilmesi için bir panel sunar.
- **Generator.js**: Sudoku tahtası oluşturma ve çözümleme mantığını barındırır.

### 7. **Data Flow**
Proje **data flow** açısından **unidirectional** yani tek yönlü veri akışı kullanmaktadır:
- **State**: `board`, `selectedNumber`, `selectedCell`, `incorrectCells`, gibi değerler ana component'ten (App.js) alt component'lere `props` ile iletilir.
- **Props**: Örneğin, `SudokuTable` bileşeni, `board`, `selectedNumber`, `incorrectCells`, gibi verileri **props** olarak alır.
  - Örnek:
    ```javascript
    <SudokuTable
      originalTable={initialBoard}
      board={board}
      transpozed={transpozed}
      selectedNumber={selectedNumber}
      selectedCell={selectedCell}
      onCellClick={handleCellClick}
      incorrectCells={incorrectCells}
    />
    ```

### 8. **Custom Algorithm for Sudoku Generation**
Projede Sudoku tahtası, **backtracking** algoritması kullanılarak çözülür ve kullanıcıya sunulur. Bu algoritma, boş hücrelere sayı yerleştirirken her adımda doğru çözüme ulaşmak için deneme-yanılma yöntemini kullanır. Sudoku çözümü için kullanılan temel algoritmalar:
- **Sudoku çözümü**: `solveSudoku(board)`
- **Blokları doldurma**: `fillDiagonalBlocks(board)`
- **Hücreleri kaldırma (boş hücre bırakma)**: `removeCells(board, clues = 35)`

---

## 🔧 İleri Dönük İyileştirmeler

- **Timer ve Skor Sistemi**: Oyuncunun ne kadar süre içinde Sudoku'yu çözdüğünü gösteren bir zamanlayıcı eklenebilir.
- **Zorluk Seviyeleri**: Oyuna farklı zorluk seviyeleri eklemek, örneğin "Kolay", "Orta", "Zor" gibi seçenekler sunmak.
- **Karmaşık Çözüm Algoritmaları**: Daha gelişmiş çözüm algoritmaları ile zorlayıcı seviyelerdeki Sudoku'ları çözme.


