// # RPG-2
#include "pch.h"
#include <list>
#include <iostream>
#include <cstdlib>
#include <string>
#include <time.h>



using namespace std;

class TheGame {
public:
	
	int money;
	int partyexp;
	int partylvl;

	void Start();

	void sqare() {};

	void newGame();
	void loadGame();
	void loopGame();

	int Forward();

	int battle(int ssexp);

	int shop(int smoney);

	int treasure(int smoney);

	int eventu() ;

	void partylvlup();

	void selectClass();

	int target();

	void inventory();
};


class Entity {
public:
	short thp;
	short hp;
	short str;
	short mag;
	short dex;
	short spd;
	short def;
	short res;
	bool live;
	short Wstr;
	short Wdex;
	short Wspd;
	short Wdef;
	short Wmag;
	short Wres;
	string name;
	virtual void entity(short sthp, short shp, short sstr, short smag, short sdex, short sdef,  short sres) {
		short thp = sthp;
		short hp = shp;
		short str = sstr;
		short mag = smag;
		short dex = sdex;
		short def = sdef;
		short res = sres;
		bool live = 1;
	};

	int getHit() {
		srand(time(NULL));
		return rand() % 100 + (dex * 3);
	}
	int getMdmg() {
		int gdmg = mag + Wmag;
		return gdmg;
	}
	int getDmg() {
		int gdmg = str + Wstr;
		return gdmg;
	}
	int recDmg(short shit, short sdmg) {
		cout << endl;
		if (shit > (spd * 2)) {
			int fhp = hp + def;
			if (sdmg < fhp) {
				if (def >= sdmg) {
					cout << "0 daños!";
				}
				else {
					hp = hp - (sdmg - def);
					cout << sdmg - def << " danios";
				}
			}
			else {
				hp = 0;
				live = 0;
				cout << sdmg - def << " danios, daño letal!";
			}
		}
		else
		{
			cout << "Ataque evadido!";
		}
		return live;
	}
	int recMdmg(short shit, short sdmg) {
		cout << endl;
		if (shit > (spd * 3)) {
			int fhp = hp + res;
			if (sdmg < fhp) {
				if (res >= sdmg) {
					cout << "0 daños!";
				}
				else {
					hp = hp - (sdmg - res);
					cout << sdmg - res << " danios";
				}
			}
			else {
				live = 0;
				hp = 0;
				cout << sdmg - res << " danios, denio letal!";
			}
		}
		else
		{
			cout << "Hechizo evadido!";
		}
		return live;
	}
	int sp1() {

		if ((hp + res) > getMdmg()) {
			hp = hp - getMdmg() + res;
		}
		return hp;
	}
	void equip(short elvl) {
		int weaponStat = elvl * 4;
		Wstr = weaponStat;
		Wdex = weaponStat;
		Wspd = weaponStat;
		Wdef = weaponStat;
		Wmag = weaponStat;
		Wres = weaponStat;
	}
	class inventario : public TheGame {
		int nWood;
		
	};
};
	
class PJ : public Entity {
public:
	short tmp;
	short mp;
	short lvl;
	short love;
	short hel;
	short ghp;
	short gstr;
	short gmag;
	short gdex;
	short gdef;
	short gres;
	short gmp;
	/*
	PJ(short sthp, short shp, short sstr, short smag, short sdex, short sdef, short sres, short stmp, short smp, short sghp, short sgstr, short sgmag, short sgdex, short sgdef, short sgres, short sgmp) {
	 thp = sthp;
	 hp = shp;
	 str = sstr;
	 mag = smag;
	 dex = sdex;
	 def = sdef;
	 res = sres;
	 live = 1;
	 tmp = stmp;
	 mp = smp;
	 lvl = 0;
	 love = 0;
	 ghp = sghp;
	 gstr = sgstr;
	 gmag = sgmag;
	 gdex = sgdex;
	 gdef = sgdef;
	 gres = sgres;
	 gmp = sgmp;
	}
	*/
	PJ(short sthp, short sstr, short smag, short sdex, short sspd, short sdef, short sres, short stmp, string snom) {
		thp = sthp;
		hp = thp;
		str = sstr;
		mag = smag;
		dex = sdex;
		spd = sspd;
		def = sdef;
		res = sres;
		hel = 3;
		live = 1;
		tmp = stmp;
		mp = tmp;
		lvl = 0;
		love = 0;
		name = snom;
		Wstr = 1;
		Wdex = 1;
		Wspd = 1;
		Wdef = 1;
		Wmag = 1;
		Wres = 1;
	}
	int magMenu() {
		short s;
		cout << "Escoge el nivel del hehcizo" << endl;
		cin >> s;
		bool im = 1;
		while ( im = 1 )
		{
			switch (s) {
			case 1:
				if (mp >= 3) {
					im = 0;
					sp1();

				}
				else
					cout << "No tienes suficiente mana." << endl;
				break;
			case 2:
				if (mp >= 3) {
					im = 0;
					sp1();
				}
				break;
			case 3:
				if (mp >= 3) {
					im = 0;
					sp1();
				}
				break;
			case 4:
				if (mp >= 3) {
					im = 0;
					sp1();
				}
				break;
			case 5:
				if (mp >= 3) {
					im = 0;
					sp1();
				}
				break;
			}

		}
		
	};
	int getHeal() {
		srand(time(NULL));
		return rand() % hel + hel;
	}
	void recHeal(short sheal) {
		cout << endl << "curado por " << sheal << " hp.";
		if ((thp - hp) >= sheal ){
			hp = hp + sheal;
		}
		else {
			hp = thp;
		}
		live = 1;
	}
	void lvlup();
};

class Enemy : public Entity {
public:
	int expg;
	Enemy(short sthp, short shp, short sstr, short smag, short sdex, short sdef, short sres, short sspd, short sexp) {
		thp = sthp;
		hp = shp;
		str = sstr;  
		mag = smag;  
		dex = sdex;  
		def = sdef;  
		res = sres;  
		spd = sspd;
		expg = sexp;
		live = 1;
	};
	

};

class Equipement {
public:
	int Qlt;
	int Prc;
	Equipement(int sQlt) {
		Qlt = sQlt;
	}
};

	PJ* pj0 = new PJ(15, 6, 6, 6, 6, 6, 6, 12, "Hero");            //Protag
	PJ* pj1 = new PJ(15, 3, 9, 6, 7, 4, 7, 18, "Pris");            //Wizard
	PJ* pj2 = new PJ(18, 8, 3, 6, 3, 9, 6, 6, "Lapp");            //Fighter
	PJ* pj3 = new PJ(15, 7, 4, 8, 8, 4, 5, 8, "Saif");            //Rougue
	PJ* pj4 = new PJ(15, 7, 7, 7, 3, 6, 6, 14, "Lana");            //Warlock
	PJ* pj5 = new PJ(18, 9, 3, 6, 7, 5, 5, 6, "Emmy");            //Barbarian
	PJ* pj6 = new PJ(15, 3, 8, 6, 4, 8, 7, 16, "Fran");            //Warmage
		  //      hp, str, mag, dex, spd, def, res, mp
	PJ* ppj[3];

	list <Equipement*> equipments;

void TheGame::loopGame() {
	do {
		Forward();
	} while (partyexp < 500);
	cout << endl << endl << "Has atravesado el bosque de los huesos.";
	cout << endl << endl << "Que espera del otro lado? iba a ser un jefe pero todo salio mal y lo lamento mucho. ";
	cout << endl << endl << "La siguiente vez entregare algo presentable, con suerte habra siguiente. Dele #1 a Simeon y grasias por haber leido esto";
}
void TheGame::newGame() {
	partyexp = 50;
	cout << "Te armas de valor de explorar el bosque de hueso. Otros aventureros te acompañaran, pero tu decidiras quienes te acompañan en el frente \n";
	selectClass();
	loopGame();
};
void TheGame::loadGame() {

}
int TheGame::Forward() {
	srand(time(NULL));
	short found = rand() % 4;
	switch (found) {
	case 1:
		partyexp = battle(partyexp);
		break;
	case 2:
		money = shop(money);
		break;
	case 3:
		money = treasure(money);
		break;
	case 4:
		eventu();
		break;
	}
	return 0;
};
void TheGame::selectClass() {
	
	cout << "\nArma tu party! \n";
	short x = 0;
	 
	for (short i = 1; i <= 4; i++) {
		cout << "\n (1) Tu mismo! \n (2) Priscilia, la maga novicia \n (3) Lapp, el hombre acorazado \n (4) Said, el escabullizo ladron \n (5) Lana, la dama enigmatica \n (6) Emmy, la fuerza titanica \n (7) Fran, el caballero arcano \n";
		cin >> x;
		switch (x) {
		case 1:
			ppj[i] = pj0;
			break;
		case 2:
			ppj[i] = pj1;
			break;
		case 3:
			ppj[i] = pj2;
			break;
		case 4:
			ppj[i] = pj3;
			break;
		case 5:
			ppj[i] = pj4;
			break;
		case 6:
			ppj[i] = pj5;
			break;
		case 7:
			ppj[i] = pj6;
			break;
		}
	}
};
void TheGame::Start() {
	system("pause");
	newGame();

}
int TheGame::target() {
	srand(time(NULL));
	short target = rand() % 2;
	if (target == 1) {
		srand(time(NULL));
		target = rand() % 4;
	}
	else
	{
		srand(time(NULL));
		target = rand() % 2 + 2;
	}
	return target;
}
int TheGame::battle(int ssexp) {
	int endb = 0;
	int stat = partyexp / 15;
	int i = 1;
	int n = 1;
	Enemy* mob[3];
	mob[1] = new Enemy(stat * 3, stat * 3, stat, stat, stat, stat, stat, stat, stat * 2);
	mob[2] = new Enemy(stat * 3, stat * 3, stat, stat, stat, stat, stat, stat, stat * 2);
	mob[3] = new Enemy(stat * 3, stat * 3, stat, stat, stat, stat, stat, stat, stat * 2);
	cout << "Un grupo de esqueletos atacan!" << endl;
	do {
		//player turn
		for (i = 1; i <= 4; i++) {
			if (ppj[i]->live == 1) {
				cout << "Turno de " << ppj[i]->name;
				cout << endl << endl << "Que hara?" << endl << "(1) Atacar" << endl << "(2) Magia" << endl << "(3) Curar heridas" << endl << "(4) Escapar";
				cin >> n;
				cout << endl;
				switch (n) {
				case 1:
					cout << "Escoge tu objetivo" << endl << "(1) Calaca" << endl << "(2) Calaca" << endl << "(3) Calaca";
					cin >> n;
					mob[n]->live = mob[n]->recDmg(ppj[i]->getHit(), ppj[i]->getDmg());
					break;
				case 2:
					cout << "Escoge tu objetivo" << endl << "(1) Calaca" << endl << "(2) Calaca" << endl << "(3) Calaca";
					cin >> n;
					mob[n]->live = mob[n]->recMdmg(ppj[i]->getHit(), ppj[i]->getMdmg());
					break;
				case 3:
					cout << "Escoge tu objetivo" << endl << ppj[1]->name << endl << ppj[2]->name << endl << ppj[3]->name << endl << ppj[4]->name;
					cin >> n;
					ppj[n]->recHeal(ppj[i]->getHeal());
					break;
				case 4:
					if ((rand()%100 + ppj[i]->dex) > 50) {
						cout << "La party ha escapado";
						return ssexp;
					}
					break;
				}
			}
			else { cout << ppj[i]<<" se encuentra inconciente"; }
		}
		//enemy turn
		
		for (i = 1; i <= 3; i++) {
			if (mob[i]->live == 1) {
				cout << endl << endl << "Turno de calaca " << i << endl;
				ppj[target()]->live = ppj[1]->recDmg(mob[i]->getHit(), mob[1]->getDmg());
			}
		}
		//Checando si siguen vivos para determinar si el loop se mantiene o si se termina la pelea.

		if (mob[1]->live == 0 && mob[2]->live == 0 && mob[3]->live == 0) {
			endb = 1;
		}
		if (ppj[1]->live == 0 && ppj[2]->live == 0 && ppj[3]->live == 0 && ppj[4]->live == 0) {
			endb = 2;
		}
	} while (endb == 0);
	if (endb == 2) {
		cout << "La party ha caido";
	}
	for (int i = 1; i <= 3; i++) {
		ssexp = ssexp + mob[i]->expg;
		delete mob[i];
	}
	partylvlup();
	return ssexp;
};
int TheGame::shop(int smoney) {
	cout << "uy una calaca que vende cosas" << endl << " (1) comprar equipo +1  (50 oros)" << endl << " (2) comprar equipo +2  (250 oros)";
	cout << endl << " (3) comprar equipo +3 (500 oros)" << endl << " (4) salir " ;
	int n = 0;
	cin >> n;
	
	return smoney;
};
void PJ::lvlup() {
	thp = (thp / 5) + 3 + thp;
	str = (str / 3) + 1 + str;
	mag = (mag / 3) + 1 + mag;
	dex = (dex / 3) + 1 + dex;
	spd = (spd / 3) + 1 + spd;
	def = (def / 3) + 1 + def;
	res = (res / 3) + 1 + res;
	hel = (hel / 3) + 1 + hel;
	tmp = mag * 2;
	lvl++;
	mp = tmp;
	hp = thp;
}
void TheGame::partylvlup() {
	if (partyexp >= (partylvl*50)) {
		cout << "La party sube de nivel!";
		pj0->lvlup();
		pj1->lvlup();
		pj2->lvlup();
		pj3->lvlup();
		pj4->lvlup();
		pj5->lvlup();
		pj6->lvlup();
		partyexp = partyexp - partylvl * 50;
	}
}
void TheGame::inventory() {
	cout << "Inventario: endl" << endl;
	for (int i = 0; i < equipments.size(); i++) {
		//cout << equipments;
		cout << endl << i;
	}
	cout << "pa quien";
	int n;
	cin >> n;
}
int TheGame::treasure(int smoney) {
	cout << "Encontraste restos de una expedicion menos afortunada" << endl;
	system("pause");
	srand(time(NULL));
	short lucky = rand() % 100 + 50;
	cout << lucky <<" oros encontrados." << endl;
	equipments.push_front(new Equipement(2));
	system("pause");
	return smoney + lucky;
};

int TheGame::eventu() {
	srand(time(NULL));
	short lucky = rand() % 6;
	switch (lucky) {
	case 1:
		cout << "Hola, soy Priscilia, Archimaga novicia. Ser archimaga es algo dificil, pero ahi la llevo. Mucho gusto conocerte." << endl;
		cout << "Amistad con Pris +1" << endl;
		pj1->love++;
		break;
	case 2:
		cout << "Lapp es mi nombre. Disculpa el ruido de la armadura, pero es algo necesaria. Si necesitas algo, no dudes." << endl;
		cout << "Amistad con Lapp +1" << endl;
		pj2->love++;
		break;
	case 3:
		cout << "Soy Saif, ¿gustas un dulce? Realmente dudo que salgamos de esta, pero supongo que cada quien tiene razones para seguir." << endl;
		cout << "Amistad con Saif +1" << endl; 
		pj3->love++;
		break;
	case 4:
		cout << "...mh... ...sabias que a las libelulas gigantes les llaman caballotes del diablo? ...hacen buenas pociones... " << endl;
		cout << "Amistad con Lana +1" << endl; 
		pj4->love++;
		break;
	case 5:
		cout << "¿Te crees valiente por adentrarte aquí? Eres un tonto si crees que eres fuerte para soportar el bosque." << endl;
		cout << "Amistad con Emmy +1" << endl; 
		pj5->love++;
		break;
	case 6:
		cout << "Oh! Que descortez de mi parte esperar hasta ahora para presentarme. Soy Sir Francis Romero de la Bacalana. Suerte no muriendo!" << endl;
		cout << "Amistad con Fran +1" << endl; 
		pj6->love++;
		break;
	}
	return 0;
}

int main() {

	TheGame* game = new TheGame();
	
	game->newGame();

}


