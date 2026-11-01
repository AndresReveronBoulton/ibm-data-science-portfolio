# Cuidado resbaladizo · colección anti-bugs

Lista personal de bugs que ya me pillaron, para no repetirlos.

Formato: fecha · nombre del patrón · ejemplo mínimo · lección.

---

### 2026-10-31 · Setup antes de operación (dicts)

**Dónde pasó:** `dojo/B2_laliga/` función `calculate_points`

**El bug:**
```python
for (t1, t2), (s1, s2) in results.items():
    if s1 > s2:
        points[t1] += 3   # 💥 KeyError si points[t1] no existe
```

**El fix:**
```python
if t1 not in points:
    points[t1] = 0
```
Inicializar la clave antes de operar. Siempre. No solo en algunos casos.

**Alternativas elegantes (para después):** `dict.get(k, 0)` · `collections.defaultdict(int)`

---

### 2026-10-31 · Indentación como sintaxis

**Dónde pasó:** `dojo/B1_birdspecies/` método `top_countries_dict`

**El bug:**
```python
def top_countries_dict(self, n):
top_names = sorted(...)   # ❌ IndentationError
```
El cuerpo del método estaba al mismo nivel que `def` en vez de 4 espacios más adentro.

**La lección:** en Python la indentación ES la sintaxis. Donde en otros lenguajes hay `{}`, aquí hay espacios. Cada bloque nuevo = +4 espacios.

---

### 2026-10-31 · Separador de argumentos Excel español

**Dónde pasó:** generar líneas Python desde hoja de cálculo

**El bug:** `=CONCAT("", B5, "': ", C5, ",")` daba error en Excel español.

**El fix:** cambiar `,` por `;` entre argumentos. En Excel español/europeo el separador es `;`, no `,`.
=CONCAT(“ ‘”; B5; “’: “; C5; “,”)


**Alternativa universal:** usar `&` en vez de `CONCAT` — nunca da líos:
=" ‘" & B5 & “’: “ & C5 & “,”