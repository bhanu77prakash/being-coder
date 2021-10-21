
struct NODE {
		int key;
		struct NODE *left;
		struct NODE *right;
		struct NODE *parent;
	    };
	    
typedef struct NODE node;


void rebuild(node *v,int s);
int store(node *v,int A[],int idx);
void destroy(node *t);
int postorder(node *t,int Size);
void inorder(node *t);
int size(node *t);
node *create(int A[],int left,int right,int median);
node *insertKey(node *t,int Key,int n,int m);
int height(node *t);


int main()
{
	node *t;
	int nsml,Key,n,m,i,h=0;
	t=(node *)malloc(sizeof(node));
	t->left=NULL;
	t->right=NULL;
	t->parent=NULL;
	printf("nsml = ");
	scanf("%d",&nsml);
	scanf("%d",&Key);
	t->key=Key;
	n=1;
	m=1;
	printf("Height = %d :",h);
	printf("\t%d\n",t->key);
	for(i=1;i<nsml;i++)
	{
		scanf("%d",&Key);
		t = insertKey(t,Key,n,m);
		h=height(t);
		printf("Height = %d",h);
		n++;
		if(m<n)
			m=n;
		inorder(t);
	}
}
	
int size(node *t)
{
	int Size=0;
	node *p;
	p=t;
	Size = postorder(p,Size);
	return Size;
}	
	
void inorder(node *t)		//inorder traversal
{
	node *p;
	p=t;
	if(p->left!=NULL)
		inorder(p->left);
	printf("%d ",p->key);
	if(p->right!=NULL)
		inorder(p->right);
	
}

int postorder(node *t,int Size)
{
	node *p;
	p=t;
	if(p->left!=NULL)
		Size = postorder(p->left,Size);
	if(p->right!=NULL)
		Size = postorder(p->right,Size);
	Size++;
	return Size;
}


void destroy(node *t)
{
	if(t->left!=NULL)
		destroy(t->left);
	if(t->right!=NULL)
		destroy(t->right);
	free(t);
}

int store(node *v,int A[],int idx)
{
	if(v->left!=NULL)
		idx = store(v->left,A,idx);
	A[idx]=v->key;
	idx++;
	if(v->right!=NULL)
		idx =store(v->right,A,idx);
	return idx;
}	


int height(node *t)
{
	int h1,h2;
	h1=h2=0;
	h1=height(t->left);
	h2=height(t->right);
	if(h1>h2)
		return h1;
	else 
		return h2;
}
	
	
void rebuild(node *v,int s)
{
	int A[s],a;
	node *reference;
	store(v,A,0);
	reference=v->parent;
	if(reference->key>v->key)
		a=1;
	else
		a=2;
	destroy(v);
	if(a==1)
		reference->left=create(A,0,s-1,s/2);
	else
		reference->right=create(A,0,s-1,s/2);	
}
	
node *create(int A[],int left,int right,int median)
{
	node *p;
	p=(node *)malloc(sizeof(node));
	p->key=A[median];
	p->left = create(A,left,median-1,(median+left)/2);
	p->right = create(A,median+1,right,(right+median)/2+1);
	return p;
}

node *insertKey(node *t,int Key,int n,int m)
{
	int depth=0;
	node *p,*q,*k;
	q=t;
	while(q!=NULL)
	{
		if(Key<q->key)
		{
			if(q->left==NULL)
			{
				p = (node *)malloc(sizeof(node));
				p->left=NULL;
				p->right=NULL;
				p->parent = q;
				p->key=Key;
				q->left=p;
				depth++;
				q=p->left;
				k=p;	
			}
			else 
			{
				k=q;
				q=q->left;
				depth++;
			}
		}
		else if(Key>q->key)
		{
			if(q->right==NULL)
			{
				p = (node *)malloc(sizeof(node));
				p->left=NULL;
				p->right=NULL;
				p->parent = q;
				p->key=Key;
				q->right=p;
				depth++;
				q=p->right;
				k=p;
			}
			else 
			{	
				k=q;	
				q=q->right;
				depth++;
			}
		}
		else 
			break;
	}	
	if(depth<=1+ceil(log((double)n)/log(3/2)))
	{
		return t;
	}
	else
		while(k!=NULL)
		{
			if(size(k)>=(2/3)*size(k->left)&&size(k)>=(2/3)*size(k->right))
			{
				rebuild(k,size(k));
				break;
			}
			else
				k=k->parent;
		}
	return t;
		
	# Added more code at the main repo
	
}



