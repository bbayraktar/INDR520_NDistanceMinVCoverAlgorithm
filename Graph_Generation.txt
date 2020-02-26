import java.util.*;
import java.util.Random;

public class GraphGenerate {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Scanner input= new Scanner(System.in);
		int node=100;
		int zValue=0;  //0
		int[][] cMatrix;
		
		System.out.println("Please enter the number of nodes:");
		//node=input.nextInt();  //number of nodes to be considered will be asked to the user.
		
		cMatrix=new int[node][node];
		
		for(int i=0; i<node; i++) {
			for(int j=0; j<node; j++) {
				cMatrix[i][j]=zValue;   //0 for one of the pairs and diagonal pairs.
			}
		}
		
		double [] randomize = new double[node];
		int [] a = new int[node];
		for(int i=0; i<node; i++) {
			for(int j=0; j<i ; j++) {
				randomize [j] = Math.random();
				if(randomize[j]>0.95) {a[j]=1;} else {a[j]=0;}
				cMatrix[j][i]=a[j];			
			}
		}
		
		int [] sum = new int[node];
		for(int i=0; i<node; i++) {
			for(int j=0; j<i ; j++) {
				sum [i] = sum [i]+cMatrix[j][i];
				//System.out.print(cMatrix[j][i]+ " ");			
			} 
			if (sum [i]<1) {
				int c = (int) (Math.random()*i);
				cMatrix[c][i] = 1;
			}
				
		}
		
		int [] sum1 = new int[node];
		for(int i=0; i<node; i++) {
			for(int j=0; j<i ; j++) {
				sum1 [i] = sum1 [i]+cMatrix[j][i];
				//System.out.print(cMatrix[j][i]+ " ");			
			}//System.out.println();	
		}
		
		int edge=-1;   //number of edges the graph has
		for(int i=0; i<node; i++) {
			for(int j=0; j<node; j++) {
				if (cMatrix[i][j]!=zValue)  //if there is an edge between two nodes, count it.
				edge=edge+1;
			}
		}
		//System.out.println(edge);
		
		System.out.println();
		for(int i=0; i<node; i++) {
			for(int j=0; j<node; j++) {
				//System.out.print(cMatrix[j][i]+ " ");
			}//System.out.println();
		}
		
		for(int i=0;i<node;i++) {
			for(int j=0;j<i;j++){
				if(cMatrix[j][i]==1) {
					System.out.print("{"+j+","+i+"}"+",");
				}
			}
			System.out.println();
		}
			
	}

}