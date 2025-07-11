<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Suivi des Macros</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    .bar { margin: 10px 0; height: 25px; background: #eee; border-radius: 5px; overflow: hidden; }
    .bar-fill { height: 100%; text-align: right; padding-right: 5px; color: white; font-weight: bold; }
    .calories { background: #ff6666; }
    .proteins { background: #66cc66; }
    .fats { background: #ffcc66; }
    .carbs { background: #66b3ff; }
    .fibers { background: #cc99ff; }
    input { margin: 5px; width: 80px; }
  </style>
</head>
<body>
  <h1>Suivi des Objectifs de Macros</h1>

  <div>
    <label>Calories: <span id="calories-left">2000</span> kcal</label>
    <div class="bar"><div class="bar-fill calories" id="calories-bar" style="width: 100%">2000</div></div>
    
    <label>Protéines: <span id="proteins-left">136</span> g</label>
    <div class="bar"><div class="bar-fill proteins" id="proteins-bar" style="width: 100%">136g</div></div>

    <label>Fibres: <span id="fibers-left">36</span> g</label>
    <div class="bar"><div class="bar-fill fibers" id="fibers-bar" style="width: 100%">36g</div></div>

    <label>Lipides: <span id="fats-left">130</span> g</label>
    <div class="bar"><div class="bar-fill fats" id="fats-bar" style="width: 100%">130g</div></div>

    <label>Glucides: <span id="carbs-left">130</span> g</label>
    <div class="bar"><div class="bar-fill carbs" id="carbs-bar" style="width: 100%">130g</div></div>
  </div>

  <h2>Ajouter un repas</h2>
  <form id="meal-form">
    <input type="number" placeholder="Calories" id="in-calories" step="1">
    <input type="number" placeholder="Protéines (g)" id="in-proteins" step="0.1">
    <input type="number" placeholder="Fibres (g)" id="in-fibers" step="0.1">
    <input type="number" placeholder="Lipides (g)" id="in-fats" step="0.1">
    <input type="number" placeholder="Glucides (g)" id="in-carbs" step="0.1">
    <button type="submit">Ajouter</button>
    <button type="button" onclick="reset()">Réinitialiser</button>
  </form>

  <script>
    const goals = {
      calories: 2000,
      proteins: 136,
      fibers: 36,
      fats: 130,
      carbs: 130
    };

    let remaining = { ...goals };

    function updateBars() {
      for (let key in goals) {
        const left = remaining[key] < 0 ? 0 : remaining[key];
        const percent = Math.max(0, (left / goals[key]) * 100);
        document.getElementById(`${key}-left`).textContent = left.toFixed(1).replace('.0','');
        const bar = document.getElementById(`${key}-bar`);
        bar.style.width = `${percent}%`;
        bar.textContent = `${left.toFixed(0)}${key === 'calories' ? '' : 'g'}`;
      }
    }

    document.getElementById('meal-form').addEventListener('submit', function (e) {
      e.preventDefault();
      remaining.calories -= parseFloat(document.getElementById('in-calories').value) || 0;
      remaining.proteins -= parseFloat(document.getElementById('in-proteins').value) || 0;
      remaining.fibers -= parseFloat(document.getElementById('in-fibers').value) || 0;
      remaining.fats -= parseFloat(document.getElementById('in-fats').value) || 0;
      remaining.carbs -= parseFloat(document.getElementById('in-carbs').value) || 0;
      updateBars();
      this.reset();
    });

    function reset() {
      remaining = { ...goals };
      updateBars();
    }

    updateBars();
  </script>
</body>
</html>
