
public class Algorithm2
{
	public static int[][] ReducedEdges(int[] nodes, int[][] edges, int k)
	{
		int[] 	vp  = {};
		int[]   vp2 = {};
		int[][] ep  = {};
		int[][] ep2 = {};
		int[] 	ep3 = {};
		int[] 	ep4 = {};
		
		if(edges.length==0) 
		{
			int[][] turn=new int [1][1];
			turn[0][0]=1;
			return turn;
		}
		
		int[][] red_edges = new int[edges.length][edges[0].length];
		for (int i=0;i<edges.length;i++)
		{
			for (int j=0; j<edges[0].length;j++)
			{
				red_edges[i][j]=edges[i][j];
			}
		}
		
		int[][] vpdel;
		
		int e=value(k)-1;
		int[] ntrail = {};
		int[] endpoints = {};
		int[] degree = {};
		int o=0;
		
		while(o==0)
		{
			o=1;
			ntrail = Algorithm1.NTrail(nodes,red_edges,e+1); //Algorithm to Find N-Trail
			if(ntrail[0]==-1)
			{
				System.out.println("No Trail with " + (e+1) + " nodes.");
				e=e-1;
				o=0;
				continue;
			}
			System.out.println();
			System.out.println(e+"-Trail:");
			for(int i=0;i<e;i++)
			{
				if(i!=e-1)
				{System.out.print(ntrail[i]+"-");}
				else
				{System.out.println(ntrail[i]);}
			}
			System.out.println();
			
		
			degree  = new int[nodes.length];
			
			for(int i=0;i<red_edges.length;i++)
			{
				degree[red_edges[i][0]]++;
				degree[red_edges[i][1]]++;
			}
			
			int[] degreeEnd  = new int[nodes.length];
			endpoints = new int[2];
			int counterDeg=0;
			
			for(int i=0;i<ntrail.length;i++)
			{
				degreeEnd[red_edges[ntrail[i]][0]]++;
				degreeEnd[red_edges[ntrail[i]][1]]++;
			}
			for(int h=0;h<nodes.length;h++)
			{
				if(degreeEnd[h]==1)
				{
					endpoints[counterDeg]=h;
					System.out.print(h + "   ");
					counterDeg++;
				}
			}
			if(counterDeg!=2)
			{
				System.out.println("There is a cycle!");
				e=e-1;
				continue;
			}
		}
			if(e<2) 
			{ 
				int[][] turn=new int [1][1];
				turn[0][0]=1;
				return turn;
			}
			System.out.println();
			System.out.println("**");
			
			vpdel=vPrime(e+1, red_edges, nodes, endpoints[0], degree);
			vp=new int[vpdel[0][0]];
			for(int i=0; i<vp.length;i++){vp[i]=vpdel[0][i+1];}
			ep3=new int[vpdel[1][0]];
			for(int i=0; i<ep3.length;i++){ep3[i]=vpdel[1][i+1];}
			
			vpdel=vPrime(e+1, red_edges, nodes, endpoints[1], degree);
			vp2=new int[vpdel[0][0]];
			for(int i=0; i<vp2.length;i++){vp2[i]=vpdel[0][i+1];}
			ep4=new int[vpdel[1][0]];
			for(int i=0; i<ep4.length;i++){ep4[i]=vpdel[1][i+1];}
			
			for(int i=0; i<vp.length;i++)
			{
				System.out.print(vp[i]+"-");
			}System.out.println();
			for(int i=0; i<ep3.length;i++)
			{
				System.out.print(ep3[i]+"-");
			}System.out.println();
			for(int i=0; i<vp2.length;i++)
			{
				System.out.print(vp2[i]+"-");
			}System.out.println();
			for(int i=0; i<ep4.length;i++)
			{
				System.out.print(ep4[i]+"-");
			}
			
			int[] h= {-1};
			int counter=0;
			for(int i=0;i<vp.length;i++)
			{
				for(int j=0;j<h.length;j++)
				{
					if(vp[i]==h[j])
					{counter++;}
				}
				if(counter==0 && vp[i]!=endpoints[0])
				{
					int[] h2=new int[h.length];
					h2=AddMatrix(h,vp[i]);
					h=Matrixvalue(h2);
				}
				counter=0;
			}
			h=DelMatrix(h);
			vp=Matrixvalue(h);
			
			int[] g= {-1};
			counter=0;
			for(int i=0;i<vp2.length;i++)
			{
				for(int j=0;j<g.length;j++)
				{
					if(vp2[i]==g[j])
					{counter++;}
				}
				if(counter==0&& vp2[i]!=endpoints[1])
				{
					int[] g2=new int[g.length];
					g2=AddMatrix(g,vp2[i]);
					g=Matrixvalue(g2);
				}
				counter=0;
			}
			g=DelMatrix(g);
			vp2=Matrixvalue(g);
			
			
			ep=ePrime(vp,endpoints[0]);
			ep2=ePrime(vp2,endpoints[1]);
			red_edges=edgeManipulation(edges,ep,ep2,ep3,ep4);
			for(int i=0;i<red_edges.length;i++)
			{
				for(int j=0; j<red_edges[0].length;j++)
				{
					System.out.print(red_edges[i][j]+"-");
				}
				System.out.println("a    ");
			}
			int[] u= new int[3];
			u[0]=e;u[1]=e;u[2]=e;
			red_edges=AddMatrix2(red_edges,u);
			
			
		return red_edges;
		
	}
	
	public static int[][] vPrime(int k, int[][] edges, int[] nodes, int ep, int[] degree)
	{
		int[] vp  = {};
		int[] vp2;
		int[] de = {};
		int[] de2;
		int[][] vpdel= new int[2][edges.length+nodes.length];

		int[][] red_edges = new int[edges.length][4];
		for (int i=0;i<edges.length;i++)
		{
			for (int j=0; j<edges[0].length;j++)
			{
				red_edges[i][j]=edges[i][j];
			}
			red_edges[i][2]=0;
			red_edges[i][3]=0;
		}
		
		
		int N=k-1;
		int[] vrE;
		int counter=0;
		
		int nopath=-1;
		while(nopath!=1)
		{
			int next=ep;
			int next1=-1;
			int last_edge=-1;
			
			while(true) {
				vrE=Algorithm1.find_vrE(degree, red_edges, next);
				if(vrE.length==0 && next!=ep && degree[next]==1 && next1!=ep || counter==N)
				{
					vp2=new int[vp.length+1]; vp2=AddMatrix(vp,next); vp=Matrixvalue(vp2);
					de2=new int[de.length+1]; de2=AddMatrix(de,last_edge); de=Matrixvalue(de2);
					for (int j=0;j<edges.length;j++)
					{
						red_edges[j][2]=0;
					}
					red_edges[last_edge][3]=1;
					for (int j=0;j<edges.length;j++)
					{
						if(red_edges[j][3]==1)
						red_edges[j][2]=1;
					}
					break;
				}
				else if(vrE.length==0 && next!=ep && degree[next]!=1)
				{
					de2=new int[de.length+1]; de2=AddMatrix(de,last_edge); de=Matrixvalue(de2);
					for (int j=0;j<edges.length;j++)
					{
						red_edges[j][2]=0;
					}
					red_edges[last_edge][3]=1;
					for (int j=0;j<edges.length;j++)
					{
						if(red_edges[j][3]==1)
						red_edges[j][2]=1;
					}
					break;
				}
				else if(vrE.length==0 && next!=ep && degree[next]==1 && next1==ep)
				{
					for (int j=0;j<edges.length;j++)
					{
						red_edges[j][2]=0;
					}
					red_edges[last_edge][3]=1;
					for (int j=0;j<edges.length;j++)
					{
						if(red_edges[j][3]==1)
						red_edges[j][2]=1;
					}
					break;
				}
				else if(vrE.length==0 && next==ep)
				{
					nopath=1;
					break;
				}
				else if(red_edges[vrE[0]][1]==next && counter<N)
				{
					next1=value(next);
					next=red_edges[vrE[0]][0];
					red_edges[vrE[0]][2]=1;
					last_edge=vrE[0];
					counter++;
				}
				else if(red_edges[vrE[0]][0]==next && counter<N)
				{
					next1=value(next);
					next=red_edges[vrE[0]][1];
					red_edges[vrE[0]][2]=1;
					last_edge=vrE[0];
					counter++;
				}
			}
			counter=0;
		}
		for(int i=0; i<2;i++)
		{
			for(int j=0;j<nodes.length+edges.length;j++)
			{
				vpdel[i][j]=-1;
			}
		}
		for(int i=1; i<vp.length+1;i++)
		{
			vpdel[0][i]=vp[i-1];
			vpdel[0][0]=vp.length;
		}
		for(int i=1; i<de.length;i++)
		{
			vpdel[1][i]=de[i];
			vpdel[1][0]=de.length;
		}
		
		
		return vpdel;
	}
	
	public static int[] AddMatrix(int[] matrix, int k)
	{
		int[] nmatrix=new int[matrix.length+1];
		for(int i=0; i<matrix.length;i++)
		{
			nmatrix[i]=matrix[i];
		}
		nmatrix[matrix.length]=k;
		return nmatrix;
	}
	
	public static int[][] AddMatrix2(int[][] matrix, int[] k)
	{
		int[][] nmatrix=new int[matrix.length+1][matrix[0].length];
		for(int i=0; i<matrix.length;i++)
		{
			for(int j=0;j<matrix[0].length;j++)
			{
				nmatrix[i][j]=matrix[i][j];
			}
		}
		for(int j=0;j<matrix[0].length;j++)
		{
			nmatrix[matrix.length][j]=k[j];
		}
		return nmatrix;
	}
	
	public static int[][] AddMatrix3(int[][] matrix, int[] k)
	{
		int[][] nmatrix=new int[matrix.length+1][3];
		for(int i=0; i<matrix.length;i++)
		{
			for(int j=0;j<matrix[0].length;j++)
			{
				nmatrix[i][j]=matrix[i][j];
			}
		}
		for(int j=0;j<3;j++)
		{
			nmatrix[matrix.length][j]=k[j];
		}
		return nmatrix;
	}
	
	public static int[] DelMatrix(int[] matrix)
	{
		int[] nmatrix=new int[matrix.length-1];
		for(int i=0; i<matrix.length-1;i++)
		{
			nmatrix[i]=matrix[i+1];
		}
		return nmatrix;
	}
	
	public static int[][] DelMatrix2(int[][] matrix)
	{
		int[][] nmatrix=new int[matrix.length-1][matrix[0].length];
		for(int i=0; i<matrix.length-1;i++)
		{
			for(int j=0;j<matrix[0].length;j++)
			{
				nmatrix[i][j]=matrix[i+1][j];
			}
		}
		return nmatrix;
	}
	public static int[][] DelMatrix3(int[][] matrix)
	{
		int[][] nmatrix=new int[matrix.length-1][matrix[0].length];
		for(int i=0; i<matrix.length-1;i++)
		{
			for(int j=0;j<matrix[0].length;j++)
			{
				nmatrix[i][j]=matrix[i][j];
			}
		}
		return nmatrix;
	}
	
	
	public static int value(int k)
	{
		int u=k;
		return u;
	}
	
	public static int[] Matrixvalue(int[] k)
	{
		int[] u=k;
		return u;
	}
	
	public static int[] delMinus(int[] matrix)
	{
		int place=-1;
		for(int i=0;i<matrix.length;i++)
		{
			if(matrix[i]==-1)
			{
				place=i;
				break;
			}
		}
		int[]nmatrix=new int[place];
		for(int i=0;i<place;i++)
		{
			nmatrix[i]=matrix[i];
		}
		return nmatrix;
	}
	
	public static int[] delInnocent(int[] matrix, int k)
	{
		int[] nmatrix=new int[matrix.length-1];
		for(int i=0;i<k;i++)
		{
			nmatrix[i]=matrix[i];
		}
		for(int i=k+1;i<matrix.length;i++)
		{
			nmatrix[i-1]=matrix[i];
		}
		return nmatrix;
	}
	
	public static int[][] ePrime(int[] vp, int ep)
	{
		int[][] aedges=new int[vp.length][2];
		for(int i=0;i<vp.length;i++)
		{
			aedges[i][0]=ep;
			aedges[i][1]=vp[i];
			System.out.println(aedges[i][0]+"-"+aedges[i][1]);
		}
		return aedges;
	}
	
	public static int[][] edgeManipulation(int[][] edges, int[][] ep, int[][] ep2, int[] ep3, int[] ep4)
	{
		int[][] red_edges= {{-1,-1,-1}};
		int[] add=new int[3];
		int in=0;
		for(int i=0;i<edges.length;i++)
		{
			for(int j=0;j<ep3.length;j++){if(i==ep3[j]){in=1;}}
			for(int j=0;j<ep4.length;j++){if(i==ep4[j]){in=1;}}
			
			if(in!=1)
			{
				add[0]=edges[i][0];
				add[1]=edges[i][1];
				add[2]=0;
				red_edges=AddMatrix2(red_edges,add);
			}
			in=0;
		}
		for(int i=0;i<ep.length;i++)
		{
			add[0]=ep[i][0];
			add[1]=ep[i][1];
			add[2]=1;
			red_edges=AddMatrix2(red_edges,add);
		}
		for(int i=0;i<ep2.length;i++)
		{
			for(int j=0;j<ep.length;j++)
			{
				if(ep2[i][0]==ep[j][1] && ep2[i][1]==ep[j][0])
				{	
					in=1;
				}
			}
			if(in!=1)
			{
				add[0]=ep2[i][0];
				add[1]=ep2[i][1];
				add[2]=1;
				red_edges=AddMatrix2(red_edges,add);
			}
			in=0;
		}
		red_edges=DelMatrix2(red_edges);
		System.out.println();
		System.out.println();
		for(int i=0;i<red_edges.length;i++)
		{
			System.out.println(red_edges[i][0]+"-"+red_edges[i][1]+": "+red_edges[i][2]);
		}
		
		return red_edges;
	}
	
	public static int NoPath(int i, int k, int j, int counter, int d)
	{
		int nopath=0;
		if((i==k-1 && j==1 && counter==0) || (i==k-1 && j==1 && d!=0 && counter==1)){nopath=1;}
		return nopath;
	}
}
