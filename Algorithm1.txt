
public class Algorithm1 
{
	public static int[] NTrail(int[] nodes, int[][] edges , int k)
	{
		int[] ntrail = {};

		int[][] resid_edges=new int[edges.length][3];
		for(int i=0;i<edges.length;i++)
		{
			for(int j=0;j<2;j++)
			{
				resid_edges[i][j]=edges[i][j];
			}
			resid_edges[i][2]=0;
		}
		
		
		int[] degree  = new int[nodes.length];
		
		for(int i=0;i<edges.length;i++)
		{
			degree[edges[i][0]]++;
			degree[edges[i][1]]++;
		}
		
		int[] edgedeg=new int[edges.length];
		int max_deg=-1;
		int max_edge=-1;
		int nvert=-1; //random vertex with degree at least 2
		for(int i=0;i<edges.length;i++)
		{
			edgedeg[i]=degree[edges[i][0]]+degree[edges[i][1]];
			if(edgedeg[i]>max_deg)
			{
				max_deg=edgedeg[i];
				max_edge=i;
			}
		}
		
		if(degree[edges[max_edge][0]]>=degree[edges[max_edge][1]])
		{
			nvert=edges[max_edge][0];
		}
		else
		{
			nvert=edges[max_edge][1];
		}
		
		int[] vrE=find_vrE(degree,resid_edges,nvert);

		int min_deg=2*edges.length+1;
		int min_edge=-1;
		for(int i=0;i<vrE.length;i++)
		{
			if(edgedeg[vrE[i]]<min_deg)
			{
				min_deg=edgedeg[vrE[i]];
				min_edge=vrE[i];
			}
		}
		
		int[] ndegree=new int[nodes.length];
		ndegree=Algorithm2.Matrixvalue(degree);
		
		int nedge=min_edge;
		
		int[] ntrail2;
		int[] ntrailnodes2;
		
		resid_edges[nedge][2]=1;
		int[] ntrailnodes=new int[1];
		if(edges[nedge][0]==nvert)
		{
			ntrailnodes[0]=resid_edges[nedge][1];
		}
		else
		{
			ntrailnodes[0]=resid_edges[nedge][0];
		}
		
		ntrail2=new int[ntrail.length+1];
		ntrail2=Algorithm2.AddMatrix(ntrail, nedge);
		ntrail=Algorithm2.Matrixvalue(ntrail2);
		ntrailnodes2=new int[ntrailnodes.length];
		ntrailnodes2=Algorithm2.AddMatrix(ntrailnodes, nvert);
		ntrailnodes=Algorithm2.Matrixvalue(ntrailnodes2);
		
		int counter=1;
		
		general:
		while(counter<k-1)
		{
			ndegree=new int[nodes.length];
			for(int i=0;i<edges.length;i++)
			{
				if(resid_edges[i][2]!=1) 
				{
					ndegree[edges[i][0]]++;
					ndegree[edges[i][1]]++;
				}
			}
			
			vrE=find_vrE(ndegree,resid_edges,nvert);
			
			back:
			for(int i=0;i<vrE.length;i++)
			{
				if((resid_edges[vrE[i]][0]==nvert && ndegree[resid_edges[vrE[i]][1]]>=2 && resid_edges[vrE[i]][2]!=1 && k-1>2)||(resid_edges[vrE[i]][0]==nvert && resid_edges[vrE[i]][2]!=1 && k-1==2))
				{
					for(int j=0;j<ntrailnodes.length;j++)
					{
						if(ntrailnodes[j]==resid_edges[vrE[i]][1])
						{
							continue back;
						}
					}
					nvert=edges[vrE[i]][1];
					nedge=vrE[i];
					resid_edges[vrE[i]][2]=1;
					break back;
				}
				else if((resid_edges[vrE[i]][1]==nvert && ndegree[resid_edges[vrE[i]][0]]>=2 && resid_edges[vrE[i]][2]!=1)||(resid_edges[vrE[i]][1]==nvert && resid_edges[vrE[i]][2]!=1 && k-1==2))
				{
					for(int j=0;j<ntrailnodes.length;j++)
					{
						if(ntrailnodes[j]==resid_edges[vrE[i]][0])
						{
							continue back;
						}
					}
					nvert=edges[vrE[i]][0];
					nedge=vrE[i];
					resid_edges[vrE[i]][2]=1;
					break back;
				}
			}
			if(nedge==ntrail[ntrail.length-1])
			{
				break general;
			}
			ntrail2=new int[ntrail.length+1];
			ntrail2=Algorithm2.AddMatrix(ntrail, nedge);
			ntrail=Algorithm2.Matrixvalue(ntrail2);
			ntrailnodes2=new int[ntrailnodes.length];
			ntrailnodes2=Algorithm2.AddMatrix(ntrailnodes, nvert);
			ntrailnodes=Algorithm2.Matrixvalue(ntrailnodes2);
			counter++;
			
		}
		
		if(ntrail.length==k-1)
		{return ntrail;}
		else
		{int[] z= {-1};
		 return z;}
	}
	
	public static int[] find_vrE(int[] degree,int[][] edges, int vr)
	{
		int[] vrE={};
		int[] vrE2;
		for(int i=0;i<edges.length;i++)
		{
			if(edges[i][2]==1)
			{
				continue;
			}
			for(int j=0;j<2;j++)
			{
				if(edges[i][j]==vr)
				{
					vrE2=new int[vrE.length+1];
					vrE2=Algorithm2.AddMatrix(vrE, i);
					vrE=Algorithm2.Matrixvalue(vrE2);
					break;
				}
			}
		}
		return vrE;
	}
	
}
