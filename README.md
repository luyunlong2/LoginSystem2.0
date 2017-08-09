# LoginSystem2.0
JNDI、JDBC的封装、页面分页显示
--数据操作工具类
public class DBTool {
	private static Context con=null;
	private static DataSource ds=null;
	private static Connection conn=null;
	
	static{
		try {
			con=new InitialContext();
			ds=(DataSource)con.lookup("java:comp/env/aaa");
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static Connection getConnection(){
		try {
			conn=ds.getConnection();
			return conn;
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return null;
	}
	
	public static void CloseConnection(){
		if(conn!=null){
			try {
				conn.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}
	
	public static int executeUpdateDML(String sql,List<Object> list){
		try {
			PreparedStatement pst=getConnection().prepareStatement(sql);
			for(int i=0;i<list.size();i++){
				pst.setObject(i+1, list.get(i));
			}
			return pst.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			CloseConnection();
		}
		return 0;
	}
	
	public static int executeUpdateDML(String sql,int jobid){
		try {
			PreparedStatement pst=getConnection().prepareStatement(sql);
			pst.setInt(1,jobid);
			return pst.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			CloseConnection();
		}
		return 0;
	}
	
	public static int executeUpdateDML(String sql){
		try {
			PreparedStatement pst=getConnection().prepareStatement(sql);
			return pst.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			CloseConnection();
		}
		return 0;
	}
	
	public static ResultSet executeQueryDDL(String sql,List<Object> list){
		ResultSet rs=null;
		try {
			PreparedStatement pst=getConnection().prepareStatement(sql);
			for(int i=0;i<list.size();i++){
				pst.setObject(i+1, list.get(i));
			}
			return pst.executeQuery();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return rs;
	}
	
	public static ResultSet executeQueryDDL(String sql,int jobid){
		ResultSet rs=null;
		try {
			PreparedStatement pst=getConnection().prepareStatement(sql);
			pst.setInt(1, jobid);
			return pst.executeQuery();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return rs;
	}
	public static ResultSet executeQueryDDL(String sql){
		ResultSet rs=null;
		try {
			PreparedStatement pst=getConnection().prepareStatement(sql);
			return pst.executeQuery();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return rs;
	}
}
