# Bandomasis: Tic Tac Toe zaidimas...

class KryziukaiNuliukai:
    def __init__(self):
        self.lenta = [' ' for _ in range(9)]
        self.dabartinis_laimetojas = None

    def spausdinti_lenta(self):
        for eile in [self.lenta[i*3:(i+1)*3] for i in range(3)]:
            print('| ' + ' | '.join(eile) + ' |')

    @staticmethod
    def spausdinti_lentos_numerius():
        numeriu_lenta = [[str(i) for i in range(j*3, (j+1)*3)] for j in range(3)]
        for eile in numeriu_lenta:
            print('| ' + ' | '.join(eile) + ' |')

    def daryti_zenkla(self, langelis, zaidejas):
        if self.lenta[langelis] == ' ':
            self.lenta[langelis] = zaidejas
            if self.laimetojas(langelis, zaidejas):
                self.dabartinis_laimetojas = zaidejas
            return True
        return False

    def laimetojas(self, langelis, zaidejas):
        eiles_nr = langelis // 3
        eile = self.lenta[eiles_nr*3:(eiles_nr+1)*3]
        if all([vietos == zaidejas for vietos in eile]):
            return True
        stulpelio_nr = langelis % 3
        stulpelis = [self.lenta[stulpelio_nr+i*3] for i in range(3)]
        if all([vietos == zaidejas for vietos in stulpelis]):
            return True
        if langelis % 2 == 0:
            pagrindinis_istrizaines = [self.lenta[i] for i in [0, 4, 8]]
            if all([vietos == zaidejas for vietos in pagrindinis_istrizaines]):
                return True
            antrinis_istrizaines = [self.lenta[i] for i in [2, 4, 6]]
            if all([vietos == zaidejas for vietos in antrinis_istrizaines]):
                return True
        return False

    def tusciai_langeliai(self):
        return ' ' in self.lenta

    def prieinami_langeliai(self):
        return [i for i, vieta in enumerate(self.lenta) if vieta == ' ']


def zaidimo_eiga():
    zaidimas = KryziukaiNuliukai()
    X_zaidejas = 'X'
    O_zaidejas = 'O'
    dabartinis_zaidejas = X_zaidejas

    while zaidimas.tusciai_langeliai():
        zaidimas.spausdinti_lenta()
        print(f'Dabar eina {dabartinis_zaidejas}.')
        prieinami_langeliai = zaidimas.prieinami_langeliai()
        print("Galimi langeliai:", prieinami_langeliai)

        try:
            ejimas = int(input("Kur padeti savo zenkla? Pradeda X (0-8): "))
            if ejimas not in prieinami_langeliai:
                raise ValueError
        except ValueError:
            print("Neteisingas ejimas. Bandykite dar karta.")
            continue

        zaidimas.daryti_zenkla(ejimas, dabartinis_zaidejas)

        if zaidimas.dabartinis_laimetojas:
            print(f'{zaidimas.dabartinis_laimetojas} laimejo!')
            zaidimas.spausdinti_lenta()
            break

        dabartinis_zaidejas = O_zaidejas if dabartinis_zaidejas == X_zaidejas else X_zaidejas

    if not zaidimas.dabartinis_laimetojas:
        print("Lygiosios!")


if __name__ == '__main__':
    zaidimo_eiga()
