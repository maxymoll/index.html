# index.html
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Sitzplatzbuchung</title>
<style>
  body {
    font-family: Arial, sans-serif;
    padding: 20px;
  }
  .table-container {
    margin-bottom: 40px;
  }
  .table-title {
    font-weight: bold;
    margin-bottom: 10px;
  }
  .seats {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
  }
  .seat {
    width: 40px;
    height: 40px;
    background: #eee;
    border: 1px solid #999;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    user-select: none;
    border-radius: 4px;
    position: relative;
  }
  .seat.selected {
    background: #8bc34a;
    color: white;
  }
  .seat input {
    width: 100%;
    height: 100%;
    border: none;
    text-align: center;
    font-size: 12px;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
</style>
</head>
<body>

<h1>Sitzplatzbuchung</h1>

<div id="tables"></div>

<script>
  // Sitzplätze pro Tisch laut Vorgabe (inkl. Untertische A0-A4 und B1-B6)
  const tables = {
    // Tische A0 bis A4 mit 10 Plätzen
    A0: 10,
    A1: 10,
    A2: 10,
    A3: 10,
    A4: 10,

    // Tische B1 bis B6 mit 12 Plätzen
    B1: 12,
    B2: 12,
    B3: 12,
    B4: 12,
    B5: 12,
    B6: 12,

    // Andere Tische
    C1: 8,
    C2: 8,
    C3: 8,
    C4: 8,
    C5: 8,
    C6: 10,
    E1: 6,
    E2: 8,
    E3: 8,
    E4: 8,
    D1: 12,
    D2: 16,
    F0: 5,
    F1: 5,
    F2: 6,
    F3: 6,
    F4: 6,
    F5: 6,
    F6: 6,
    F7: 6,
    F8: 6,
    F9: 8,
    H0: 6,
    H1: 6,
    H3: 5,
    H4: 6,
    H5: 6,
    H6: 4,
    H7: 5,
    H8: 5,
    H9: 5,
    H10: 5,
    G1: 6,
    G2: 6,
    G3: 6,
  };

  const container = document.getElementById('tables');

  for (const [table, seatsCount] of Object.entries(tables)) {
    const tableDiv = document.createElement('div');
    tableDiv.classList.add('table-container');

    const title = document.createElement('div');
    title.classList.add('table-title');
    title.textContent = `Tisch ${table} (${seatsCount} Plätze)`;
    tableDiv.appendChild(title);

    const seatsDiv = document.createElement('div');
    seatsDiv.classList.add('seats');

    for (let i = 1; i <= seatsCount; i++) {
      const seat = document.createElement('div');
      seat.classList.add('seat');
      seat.textContent = `${table}${i}`;

      // Klick auf Sitzplatz: öffnet Eingabefeld zum Namen eintragen
      seat.addEventListener('click', () => {
        if (seat.querySelector('input')) return; // Wenn schon aktiv, nichts tun

        const currentName = seat.getAttribute('data-name') || '';
        seat.textContent = '';

        const input = document.createElement('input');
        input.type = 'text';
        input.value = currentName;
        input.placeholder = 'Name';
        seat.appendChild(input);
        input.focus();

        input.addEventListener('blur', () => {
          const val = input.value.trim();
          seat.removeChild(input);
          seat.textContent = `${table}${i}`;
          if (val.length > 0) {
            seat.setAttribute('data-name', val);
            seat.classList.add('selected');
            seat.title = `Reserviert für: ${val}`;
          } else {
            seat.removeAttribute('data-name');
            seat.classList.remove('selected');
            seat.title = '';
          }
        });

        input.addEventListener('keydown', (e) => {
          if (e.key === 'Enter') {
            input.blur();
          } else if (e.key === 'Escape') {
            input.value = '';
            input.blur();
          }
        });
      });

      seatsDiv.appendChild(seat);
    }

    tableDiv.appendChild(seatsDiv);
    container.appendChild(tableDiv);
  }
</script>

</body>
</html>
