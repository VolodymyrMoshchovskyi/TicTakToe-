# TicTakToe-

import java.util.Scanner;

public class TicTacToe {

	// fiksovani zminni
	public static final String POROZHNYA = "   ";
	public static final String KHRESTYK = " X ";
	public static final String NULYK = " O ";
	public static String aktyvnyiGravets;
	// zminni dla zberigannia info pro igrove pole i stan gry
	public static final int RYADKIV = 3, STOVPCHYKIV = 3;
	public static String[][] sitka = new String[RYADKIV][STOVPCHYKIV];
	public static int statusGry;
	public static final int STATUS_GRA_TRYVAYE=0, STATUS_NICHYYA=1, STATUS_PEREMOGA_X=3, STATUS_PEREMOGA_O=4;
	public static Scanner in = new Scanner(System.in);

	// metody
	public static void main(String[] arg) {
		PochatyGru();
		do {
			OtrymatyVvedenniaGravtsya();
			ProanalizuvatySitku();
			VyvestySitku();
			if (statusGry==STATUS_PEREMOGA_X) {
				System.out.println(" X peremig! Vitajemo! ");
			} else if (statusGry==STATUS_PEREMOGA_O) {
				System.out.println(" O peremig! Vitajemo! ");
			} else if (statusGry==STATUS_NICHYYA) {
				System.out.println(" U nas wyjshla nichyja !!! Vitaju Was oboch!");
			}

			aktyvnyiGravets = (aktyvnyiGravets==KHRESTYK?NULYK:KHRESTYK);
		}
		while (statusGry == STATUS_GRA_TRYVAYE);
	}
	public static void PochatyGru() {
		for (int ryad=0; ryad<RYADKIV; ryad++) {
			for(int stovp=0; stovp<STOVPCHYKIV; stovp++) {
				sitka[ryad][stovp]=POROZHNYA;

			}
		}
		aktyvnyiGravets = KHRESTYK;
		VyvestySitku();

	}
	public static void OtrymatyVvedenniaGravtsya() {

		boolean vvedennyaDijsne =false;
		do {
			System.out.println(" Gravets " + aktyvnyiGravets + "vvedit riadok (1-3) i stovpczyk (1-3) cherez probil");
			int ryad = in.nextInt()-1; int stovp = in.nextInt()-1;
			if (ryad>=0 && ryad < RYADKIV && stovp >=0 && stovp < STOVPCHYKIV && sitka[ryad][stovp]==POROZHNYA) {
				sitka[ryad][stovp] = aktyvnyiGravets;
				vvedennyaDijsne = true;
			} else {
				System.out.println("wybrane rozmiszczennia (" + (ryad + 1) + "," + (stovp + 1) + ") ne moze buty wykorystane. Sprobujte szcze raz...");
			}
		}
		while(!vvedennyaDijsne);
	}
	public static void ProanalizuvatySitku() {
		String peremozhets = ZnajtyPeremozhtsua();
		if (peremozhets.equals(KHRESTYK)) {
			statusGry = STATUS_PEREMOGA_X;
		} else if (peremozhets.equals(NULYK)) {
			statusGry = STATUS_PEREMOGA_O;
		} else if (CzyVsiKlitynkyZapovneni()) {
			statusGry = STATUS_NICHYYA;
		} else {
			statusGry = STATUS_GRA_TRYVAYE;
		}
	}
	public static boolean CzyVsiKlitynkyZapovneni() {
		for (int ryad = 0; ryad< RYADKIV; ryad++) {
			for (int stovp = 0; stovp<STOVPCHYKIV; stovp++) {
				if (sitka[ryad][stovp]==POROZHNYA) {
					return false;
				}
			}
		}
		return true;
	}
	public static String ZnajtyPeremozhtsua () {

		int iKilkistOdynakovykh = 0;

		// 3 v ryad
		for (int ryad = 0; ryad<RYADKIV; ryad++) {
			for (int stovp=0; stovp<STOVPCHYKIV; stovp++) {
				if (sitka[ryad][0] != POROZHNYA && sitka[ryad][0]==sitka[ryad][stovp]) {
					iKilkistOdynakovykh++;

				}
			}
			if (iKilkistOdynakovykh==3) {
				return sitka[ryad][0];
			}
		}
		// 3 v stovpchyk		
		for (int stovp = 0; stovp<STOVPCHYKIV; stovp++) {
			iKilkistOdynakovykh = 0;
			for (int ryad=0; ryad<RYADKIV; ryad++) {
				if (sitka[0][stovp] != POROZHNYA && sitka[0][stovp]==sitka[ryad][stovp]) {
					iKilkistOdynakovykh++;

				}
			}
			if (iKilkistOdynakovykh==3) {
				return sitka[0][stovp];
			}
		}
		// po diagonali
		if (sitka[0][0]!=POROZHNYA && sitka[0][0]==sitka[1][1] && sitka[0][0]==sitka[2][2]) {
			return sitka[0][0];
		}
		if (sitka[2][0]!=POROZHNYA && sitka[2][0]==sitka[1][1] && sitka[2][0]==sitka[0][2]) {
			return sitka[0][2];
		}
		return POROZHNYA;
	}
	public static void VyvestySitku() {
		for (int ryad = 0; ryad < RYADKIV; ryad++) {
			for (int stovp = 0; stovp<STOVPCHYKIV; stovp++) {
				System.out.print(sitka[ryad][stovp]);
				if (stovp!=STOVPCHYKIV-1) {
					System.out.print(" | ");
				}
			}
			System.out.println();
			if (ryad!=RYADKIV-1) {
				System.out.println("---------------");
			}
		}
		System.out.println();
	}

}
