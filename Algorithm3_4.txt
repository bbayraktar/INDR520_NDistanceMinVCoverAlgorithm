
public class Algorithm3_4
{
	public static void nDistMinVertexCover(int[][] edges, int[] nodes)
	{
		int[] degree=new int[nodes.length];
		int max_deg=-1;
		int max_node=-1;
	
		int used=0;
		int[] use=new int[edges.length];
		for(int i=0;i<edges.length;i++)
		{
			use[i]=0;
		}
		int[] maxused = {};
		int[] maxusedaux;
		
		outerloop:
		while(used==0)
		{
			used=1;
			for(int i=0;i<edges.length;i++)
			{
				if(use[i]!=1)
				{
					degree[edges[i][0]]++;
					degree[edges[i][1]]++;
					used=0;
				}
				else if(i==edges.length-1 && used!=0)
				{
					break outerloop;
				}
			}
		
			System.out.println();
			System.out.println();
			for(int i=0;i<nodes.length;i++)
			{
				if(degree[i]>=max_deg)
				{
					max_deg=degree[i];
					max_node=i;
				}
				System.out.print(degree[i]+"  "+ i);
				System.out.println();

			}
			System.out.println();
			System.out.println("maxnode: " + max_node);
			
			System.out.println();
			System.out.println();
			for(int i=0;i<edges.length;i++)
			{
				for(int j=0;j<2;j++)
				{
					if(edges[i][j]==max_node && use[i]==0)
					{
						System.out.println("om: "+edges[i][0]+"-"+edges[i][1]);
						use[i]=1;
					}
				}
			}
			
			maxusedaux=new int[maxused.length+1];
			maxusedaux=Algorithm2.AddMatrix(maxused, max_node);
			maxused=Algorithm2.Matrixvalue(maxusedaux);

			for(int h=0;h<nodes.length;h++)
			{degree[h]=0;}
			max_deg=-1;
			max_node=-1;
			
		}
		System.out.println();
		System.out.print("Approximated N-Distance Minimal Vertex Covers={");
		for(int i=0;i<maxused.length;i++)
		{
			if(i!=maxused.length-1)
			{System.out.print(maxused[i]+",");}
			else
			{System.out.print(maxused[i]);}
		}
		System.out.print("}");
		System.out.println();
		System.out.println("Vertex Cover Size:" + maxused.length);
		
	}
}
