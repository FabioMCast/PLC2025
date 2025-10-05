Possível resolução para TP2 de PLC

Fábio Mendes Castelhano, A105728<img width="122" height="131" alt="Captura de tela 2025-05-17 195824" src="https://github.com/user-attachments/assets/15db067d-2475-4d4a-b366-187cceb2ec55" />

Nesse TP, pretende-se criar um pequeno conversor de MarkDown para HTML para os elemntos descritos na "Basic Syntax" da Cheat Sheet. A seguir, encontra-se a minha versão desse conversor[TPC2.ipynb](https://github.com/user-attachments/files/22709551/TPC2.ipynb)
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 14,
   "id": "02c8cc3d",
   "metadata": {},
   "outputs": [],
   "source": [
    "import re\n",
    "\n",
    "def listasOrdenadasToHTML(texto):\n",
    "    linhas = texto.split('\\n')\n",
    "    novas_linhas = []\n",
    "    dentro_lista = False\n",
    "\n",
    "    for linha in linhas:\n",
    "        if re.match(r'^\\s*\\d+\\.\\s+', linha):\n",
    "            if not dentro_lista:\n",
    "                novas_linhas.append('<ol>')\n",
    "                dentro_lista = True\n",
    "            linha = re.sub(r'^\\s*\\d+\\.\\s+(.+)$', r'<li>\\1</li>', linha)\n",
    "            novas_linhas.append(linha)\n",
    "        else:\n",
    "            if dentro_lista:\n",
    "                novas_linhas.append('</ol>')\n",
    "                dentro_lista = False\n",
    "            novas_linhas.append(linha)\n",
    "\n",
    "    if dentro_lista:\n",
    "        novas_linhas.append('</ol>')\n",
    "\n",
    "    return '\\n'.join(novas_linhas)    \n",
    "\n",
    "def coversorMarkdownToHTML(texto):\n",
    "    #1. Cabeçalhos\n",
    "    texto = re.sub(r'^(#{1,6})\\s*(.+)$', lambda m: f\"<h{len(m.group(1))}>{m.group(2).strip()}</h{len(m.group(1))}>\", texto, flags=re.MULTILINE)\n",
    "\n",
    "    #2. Negrito\n",
    "    texto = re.sub(r'\\*\\*(.+?)\\*\\*',r'<b>\\1</b>',texto)\n",
    "\n",
    "    #3. Itálico\n",
    "    texto = re.sub(r'\\*(.+?)\\*',r'<i>\\1</i>',texto)\n",
    "\n",
    "    #4. Listas ordenadas\n",
    "    texto = listasOrdenadasToHTML(texto)\n",
    "\n",
    "    #5. Imagens\n",
    "    texto = re.sub(r'!\\[(.*)\\]\\((.+)\\)',r'<img src=\"\\2\" alt=\"\\1\"/>',texto)\n",
    "\n",
    "    #6. Links\n",
    "    texto = re.sub(r'\\[(.+?)\\]\\((.+?)\\)',r'<a href=\"\\2\">\\1</a>',texto)\n",
    "\n",
    "    return texto"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "base",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.13.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
