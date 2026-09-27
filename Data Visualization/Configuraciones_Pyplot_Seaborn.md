# Matplotlib / Seaborn - Configuraciones útiles

## Figura

``` python
plt.figure(figsize=(12,6))
plt.tight_layout()
plt.show()
plt.savefig("grafico.png", dpi=300, bbox_inches="tight")
```

------------------------------------------------------------------------

## Títulos y ejes

``` python
plt.title("Título", fontsize=18)
plt.xlabel("X", fontsize=12)
plt.ylabel("Y", fontsize=12)
```

------------------------------------------------------------------------

## Líneas de referencia

``` python
plt.axvline(x=5, color="red", linestyle="--", linewidth=2)
plt.axhline(y=10, color="blue", linestyle=":")

plt.vlines(5, ymin=0, ymax=100,
           colors="red",
           linestyle="dashed",
           linewidth=2)

plt.hlines(20, xmin=0, xmax=10,
           colors="green")
```

------------------------------------------------------------------------

## Anotaciones

``` python
plt.annotate("Texto",
             xy=(5,10))

plt.annotate("Texto",
             xy=(5,10),
             xytext=(7,15),
             arrowprops=dict(arrowstyle="->"))

plt.text(5,10,"Texto")
```

------------------------------------------------------------------------

## Colores y estilos

``` python
color="red"
color="royalblue"

linestyle="-"
linestyle="--"
linestyle=":"
linestyle="-."

linewidth=2

marker="o"
marker="s"
marker="^"
marker="x"
marker="*"

alpha=0.5
```

------------------------------------------------------------------------

## Límites

``` python
plt.xlim(0,20)
plt.ylim(0,100)
```

------------------------------------------------------------------------

## Ticks

``` python
plt.xticks(rotation=45)
plt.xticks(fontsize=12)

plt.yticks(fontsize=12)
```

------------------------------------------------------------------------

## Leyenda

``` python
plt.legend()

plt.legend(loc="best")
plt.legend(loc="upper right")
plt.legend(loc="upper left")
plt.legend(loc="lower right")
plt.legend(loc="lower left")
```

------------------------------------------------------------------------

## Grid

``` python
plt.grid(True)

plt.grid(axis="x")

plt.grid(axis="y")

plt.grid(alpha=0.3)
```

------------------------------------------------------------------------

## Seaborn

``` python
sns.set_theme()

sns.set_style("whitegrid")
sns.set_style("darkgrid")
sns.set_style("white")
sns.set_style("dark")
sns.set_style("ticks")

sns.set_context("paper")
sns.set_context("notebook")
sns.set_context("talk")
sns.set_context("poster")

sns.set_palette("deep")
sns.set_palette("pastel")
sns.set_palette("Set2")
sns.set_palette("viridis")
sns.set_palette("coolwarm")
```

------------------------------------------------------------------------

## Paletas (cmap)

``` python
cmap="viridis"
cmap="coolwarm"
cmap="Blues"
cmap="Reds"
cmap="Greens"
cmap="magma"
cmap="plasma"
```
