#include <iostream>
#include <string>
using namespace std;

typedef unsigned char byte;

void mostrarEstado(byte e) {
    cout << "Estado (bin): " << ((e >> 1) & 1) << (e & 1);
    byte n = (~(e >> 1) & ~e) & 1;
    byte l = (~(e >> 1) & e) & 1;
    byte s = ((e >> 1) & ~e) & 1;
    byte o = ((e >> 1) & e) & 1;

    cout << "Verde N:" << (int)n;
    cout << " L:" << (int)l;
    cout << " S:" << (int)s;
    cout << " O:" << (int)o << endl;
}

byte proxEstado(byte atual, byte Ncar, byte Scar, byte Ecar, byte Wcar) {
    byte n = (~(atual >> 1) & ~atual) & 1;
    byte l = (~(atual >> 1) & atual) & 1;
    byte s = ((atual >> 1) & ~atual) & 1;
    byte o = ((atual >> 1) & atual) & 1;

    byte mudaL = n & Ecar; 
    byte mudaS = l & Scar; 
    byte mudaO = s & Wcar;
    byte mudaN = o & Ncar; 

    byte ficaN = n & ~Ecar;
    byte ficaL = l & ~Scar;
    byte ficaS = s & ~Wcar;
    byte ficaO = w & ~Ncar;

    byte novoN = (ficaN | mudaN) & 1;
    byte novoL = (ficaL | mudaL) & 1;
    byte novoS = (ficaS | mudaS) & 1;
    byte novoO = (ficaO | mudaO) & 1;

    byte b1 = (novoS | novoO) & 1;
    byte b0 = (novoL | novoO) & 1;

    return ((b1 << 1) | b0) & 3;
}
byte textoParaBit(string txt) {
    return (txt == "sim") & 1;
}
int main() {
    byte estado, Ncar, Ecar, Scar, Wcar;
    string txt;

    cout << "=== SEMAFORO INTELIGENTE ===";
    cout << "01 = Norte\n02 = Leste\n03 = Sul\n04 = Oeste\n\n";

    cout << "Digite o estado atual (01 a 04): ";
    cin >> estado;
    estado = (estado - 1) & 3;

    cout << "Tem carro no Norte (sim/nao)? ";
    cin >> txt;
    Ncar = textoParaBit(txt);

    cout << "Tem carro no Leste (sim/nao)? ";
    cin >> txt;
    Ecar = textoParaBit(txt);

    cout << "Tem carro no Sul (sim/nao)? ";
    cin >> txt;
    Scar = textoParaBit(txt);

    cout << "Tem carro no Oeste (sim/nao)? ";
    cin >> txt;
    Wcar = textoParaBit(txt);

    mostrarEstado(estado);

    byte p = proxEstado(estado, Ncar, Scar, Ecar, Wcar);

    cout << "Proximo estado:";
    mostrarEstado(p);
    cout << "Estado numerico: 0" << (int)(p + 1) << endl;
    return 0;
}
