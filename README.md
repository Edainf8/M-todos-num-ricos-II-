{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "source": [
        "$$ REGLA COMPUESTA DE SIMPSON $$"
      ],
      "metadata": {
        "id": "z3OhMI0-wiNd"
      }
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "Omwk8pAZqLvL"
      },
      "outputs": [],
      "source": [
        "import sympy as sy\n",
        "import matplotlib.pyplot as plt\n",
        "import numpy as np"
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "###_Teorema_\n",
        "Sean $f \\in C^{4} [a,b]$, n par, $ h = \\frac {b-a}{n} $ y $x_j=a+jh$ para $j = 0,1,2,...,n. $ Existe $\\mu ∈ (a,b)$ tal que la regla compuesta de Simpson para n su intervalos puede escribirse como\n",
        "\n"
      ],
      "metadata": {
        "id": "VFwLWYrdRwH5"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "\n",
        " $$\\int_{a}^{b} f(x)dx \\ = \\frac {h}{3}[f(a) + 2 \\sum_{j=1}^{\\frac{n}{2} - 1} f(x_{2j}) + 4 \\sum_{j=1}^{\\frac{n}{2}} f(x_{2j-1}) + f(b)] - \\frac{b-a}{180}h^4f^{IV}(\\mu) $$"
      ],
      "metadata": {
        "id": "f_BR1F8CxMXc"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "Notemos que con los subíndices en las sumatorias $f(x_{2j})$ es par y $f(x_{2j-1})$ son impares para $j = 1,2,...,n-1$ por lo que para el codigo haremos dos acomuladores donde se iran sumando los terminos conforme sea par o impar"
      ],
      "metadata": {
        "id": "htl0wZTCtwQu"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "###_CODIGO DE LA REGLA COMPUESTA DE SIMPSON_"
      ],
      "metadata": {
        "id": "R0jY-CsWyimY"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "def RCS(f,a,b,n):\n",
        "  h = (b-a)/n\n",
        "  XI0 = f(a) + f(b)\n",
        "  XI2 = 0 #pares\n",
        "  XI1 = 0 #impares\n",
        "  for j in range(1,n):\n",
        "    X = a + j*h\n",
        "    if j % 2 == 0:\n",
        "      XI2 = XI2 + f(X)\n",
        "    else:\n",
        "      XI1 = XI1 + f(X)\n",
        "\n",
        "  XI = (h/3)*(XI0 + 2*XI2 + 4*XI1)\n",
        "  return XI #devuelve el valor de la integral sin error"
      ],
      "metadata": {
        "id": "_GdtmXW0wREP"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "### _Ejemplo_\n",
        "Aproximar Usando la Regla Compuesta de Simpson\n",
        "$$\\int_{0}^{2} e^{2x}sin(3x)dx $$"
      ],
      "metadata": {
        "id": "eKyYxdJBfhHE"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "def f(x):\n",
        "  return np.exp(2*x)*np.sin(3*x) #definimos la función\n",
        "I=RCS(f,0,2,54) #metemos la funcion con los parametros a=0 b=2 n=54 y la funcion\n",
        "print(f\"La aproximacion de f(x) es: {I}\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "jmg8Y4FBydEO",
        "outputId": "ccfa03c6-6303-4c97-eaef-cc51f75abe49"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "La aproximacion de f(x) es: -14.213964900134542\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "### _La grafica de $f(x)$_"
      ],
      "metadata": {
        "id": "EMGvZRwcglNT"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "x = np.linspace(0,3,100) # graficamos el intervalo [0,3] de la funcion para ver un poco mas de la funcion\n",
        "plt.plot(x,f(x))\n",
        "plt.ylabel('f(x)') #declaramos el eje y\n",
        "plt.xlabel('x') #declaramos el eje x\n",
        "plt.title('Grafica de f(x)', color = 'blue') #el encabezado de la grafica\n",
        "plt.axhline(y=0, color = 'black') #marcamos el eje \"y\"\n",
        "plt.axvline(x=0, color = 'black') #marcamos el eje \"x\"\n",
        "a_I=np.linspace(0,2) #asignamos esto a a_I para poder marcar el area que queremos aproximar\n",
        "plt.fill_between(a_I, f(a_I),0, alpha= 0.4, color = 'red') #marca el area que queremos aproximar\n",
        "plt.text(0.5, 200, f\"I = {I}\") # guarda el resultado de I y lo mete a la grafica"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 489
        },
        "id": "x5ylNAbEf35S",
        "outputId": "c3012165-2b78-4863-8e58-7bf150bee9b5"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Text(0.5, 200, 'I = -14.213964900134542')"
            ]
          },
          "metadata": {},
          "execution_count": 9
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "En la imagen podemos ver el area de $\\int_{0}^{2} e^{2x}sin(3x)dx$ en lo cual podemos notar que como el area pasa por debajo del eje x entonces debe ser negativo y con el resultado que se obtuvo podemos decir que tuvimos una aproximacion mas certera por lo que\n",
        "\n",
        "$$\\int_{0}^{2} e^{2x}sin(3x)dx \\approx -14.213964900134542$$"
      ],
      "metadata": {
        "id": "zch77MfFjYq8"
      }
    }
  ]
}
