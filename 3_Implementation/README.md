#include <stdio.h>

int main(int argc, char **argv){
	//Opening of two files 
	FILE *fp,*fp2;
	char inputFileName[]="meter.txt";
	char outputfile[]="power.txt";
	fp = fopen(inputFileName,"a+");
	fp2=fopen(outputfile,"a+");
	double lastMeterReading,lastBillAmount;
	
	int monthno=1;
	char ch;
	fseek(fp, 0, SEEK_END);
	
//Entering last months bill details
if (ftell(fp) == 0){
	if(monthno ==1){
	printf("Electricity Bill Tracker\n\n");
	printf("Please enter last month's meter reading:\t");
	scanf("%lf",&lastMeterReading);
	printf("Please enter total amount from last month's bill:\t");
	scanf("%lf",&lastBillAmount);
	fprintf(fp,"MonthNo=%d\tReading=%.2lf\tPayment=%.2lf\n",monthno,lastMeterReading,lastBillAmount);
	printf("\nMeter reading = %.2lf\nTotal bill is %.2lf\n",lastMeterReading,lastBillAmount);

	}
		
}else{
	
	int m,enterMonth;
	double currentMeterReading,thisMeterReading,previousMeterReading,charge;
	double rate,units;
	double amount=0;
	printf("\nMenu...");
	printf("\n1.Enter this month's reading");
	printf("\n2.Enter payment details");
	printf("\n3.View payment history");
	printf("\n4.View monthly power consumption");
	printf("\n\nType any one menu number from the above menu list to proceed\n");
	scanf("%d",&m);

	//[1]Entering this month's reading
	if(m==1){
		printf("\nEnter this Month's number:");
			scanf("%d",&enterMonth);
		printf("\nEnter this Month's meter reading:");
			scanf("%lf",&thisMeterReading);
		printf("\nPayments made =%.2lf",amount);
		fprintf(fp,"MonthNo=%d\tReading=%.2lf\tPayment(Rs.)=%.2lf\n",enterMonth,thisMeterReading,amount);
		
	//[2]Entering payment details
	}else if(m==2){
		
		printf("\nEnter Month No. to be updated :");
			scanf("%d",&enterMonth);
		printf("\nEnter previous month's meter reading :");
			scanf("%lf",&previousMeterReading);
		printf("\nEnter current Meter reading :");
			scanf("%lf",&currentMeterReading);
			
		//Calculating the total bill
		units=previousMeterReading-currentMeterReading;
		rate=units;
		if(rate>0 && rate<=30){
		charge=(units*2.50) +30;
		}else if (rate>=31 && rate<=60){
		charge=(30*2.50 + (units-30)*4.85)+30+60;
		}else if(rate>=61 && rate<=90){
		charge=(60*7.85 + (units-60)*10)+0+90;
		}else if(rate>=91 && rate<=120){
		charge=(60*7.85 + 30*10 +(units-90)*27.75)+90+480;
		}else if(rate>=121 && rate<=180){
		charge=(60*7.85 + 30*10 + 30*27.75 + (units-120)*32)+90+480+480;
		}else{
		charge=(60*7.85 + 30*10 + 30*27.75 + 30*32 +(units-150)*45)+90+480+480+540;
		}
		printf("\nBill amount for Month No.%d = Rs %.2lf",enterMonth,charge);
		fprintf(fp,"MonthNo=%d\tReading=%.2lf\tPayment(Rs.)=%.2lf\n",enterMonth,currentMeterReading,charge);
		fprintf(fp2,"%d\t\t\t%.2lf\n",enterMonth,units);
		
	//[3]Viewing Payment History
	}else if(m==3){
		fp = fopen(inputFileName, "r");
		ch = fgetc(fp);
		while (ch != EOF){
        printf ("%c", ch);
        ch = fgetc(fp);
		}
	
	//[4]Viewing Monthly Power Consumption
	}else if(m==4){	
		printf("Month No\tPower Consumed(kWh)\n");
		fp2 = fopen(outputfile,"r");
		ch = fgetc(fp2);
		while (ch != EOF){
		printf ("%c", ch);
        ch = fgetc(fp2);
			}
		}
	
	}
	return 0;
}
