package application;

import java.util.Locale;
import java.util.Scanner;

import services.InterestService;
import services.UsaInterestService;

public class Program {

	public static void main(String[] args) {

		Locale.setDefault(Locale.CANADA);
		Scanner sc = new Scanner(System.in);
		
		System.out.print("Amount: ");
		double amount = sc.nextDouble();
		System.out.print("Months: ");
		int months = sc.nextInt();
		
		InterestService is = new UsaInterestService(1.0);
		/*
		 * caso eu queira trocar aqui do USA para BR, eu posso trocar que eu não vou ter problema nenhum.
		 * 
		 * agora leia o que ta em // no br/usaInterestService..
		 */
		double payment = is.payment(amount, months);
		// e eu posso normalmente, chamar aquele metodo padrão aqui no programa principal
		System.out.println("Payment after " + months + " months: ");
		System.out.println(String.format("%.2f", payment));
		
		sc.close();
	}
}

===========================================================================

package services;

import java.security.InvalidParameterException;

public interface InterestService {

	double getInterestRate();
	
	/*
	 * aqui ainda nas duas classes tinha as duas operações, a tava de juros(BR/USAInterestRate), e o pagamento(payment)
	 * e ai que agora vai entrar o default method, olha que bacana, ao invez de colocar a implementação do pagamento
	 * em cada uma das duas clases(br/usa), eu vou copiar o payment e colar aqui dentro da INTERFACE, colocar aqui, só
	 * que ai, ao invez de public eu vou colocar default, e troco o interestRate pelo metodo getInterestRate e coloco aqui
	 * a chamada do metodo getInterestRate pra pegar a taxa de juros da classe expecifica.
	 * isso é perfitamente legal de se implementar em uma interface.
	 *  
	 * e nas clases(br/usa) eu posso apagar a implementação do metodo pagamento(payment), eu não presico sobreescrever
	 * esse metodos lá. e lá no programa principal.......
	 */

	default double payment(double amount, int months) {
		if (months < 1) {
			throw new InvalidParameterException("Months must be greater than zero");
		}
		return amount * Math.pow(1.0 + getInterestRate() / 100.0, months);
	}	
}

===========================================================================

package services;

public class BrazilInterestService implements InterestService {

	private double interestRate;
	
	public BrazilInterestService(double interestRate) {
		this.interestRate = interestRate;
	}

/*
 * 	agora o ultimo questionamento aqui e se eu não poderia colocar o getInteretRate na interface tambem como metodo padrão
 * nesse caso, eu não posso colocar, pq? pq esse metodo aqui depende do valor da variavel que está armazenada no serviço
 * como a interface não pode armazenar estado, eu não posso colocar o valor lá, então esse metodo só vai poder ficar na
 * implementação mesmo, e o construtor tambem, a interface não pode ter construtor
 */
	
	@Override
	public double getInterestRate() {
		return interestRate;
	}

}

===========================================================================

package services;

public class UsaInterestService implements InterestService {

	private double interestRate;

	public UsaInterestService(double interestRate) {
		this.interestRate = interestRate;
	}

	/*
	 * 	agora o ultimo questionamento aqui e se eu não poderia colocar o getInteretRate na interface tambem como metodo padrão
	 * nesse caso, eu não posso colocar, pq? pq esse metodo aqui depende do valor da variavel que está armazenada no serviço
	 * como a interface não pode armazenar estado, eu não posso colocar o valor lá, então esse metodo só vai poder ficar na
	 * implementação mesmo, e o construtor tambem, a interface não pode ter construtor
	 */
	
	@Override
	public double getInterestRate() {
		return interestRate;
	}
	
}
![13-interfaces pdf](https://user-images.githubusercontent.com/61166475/154861255-151ce124-5a2d-4e80-85eb-5fea7bf086c6.png)
![14-interfaces pdf](https://user-images.githubusercontent.com/61166475/154861264-0f5cdf82-68fc-4cfc-a688-c9ef61fdee0d.png)
![15-interfaces pdf](https://user-images.githubusercontent.com/61166475/154861265-07078844-6f2a-4175-bdc6-685540661337.png)
![16-interfaces pdf](https://user-images.githubusercontent.com/61166475/154861266-5caaa8cc-a26a-405d-bce2-11166cc163ed.png)
![17-interfaces pdf](https://user-images.githubusercontent.com/61166475/154861267-bfca3d1e-3b60-400b-9e8a-452ce1ed8f0b.png)
[13-interfaces.pdf](https://github.com/yarisb/default_methods/files/8104816/13-interfaces.pdf)
